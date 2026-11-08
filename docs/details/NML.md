# neurino - Neurino Markup Language
## Introduction
The Neurino Markup Language (NML) is a domain-specific language used to describe how ninos are connected. The language uses each line of the textfile to describe a stage of neural network, where information flows from top to bottom.

TODO: Give the command used to call the compiler

## Syntax
### Inline Documentation
The NML compiler ignores all occurrences of spaces `' '`, tabs `'\t'`, normal brackets `'('` `')'`, and lines without any functional syntax. This means you can freely use them to help format your NML files.
```nml
(embedding)
```
Comments are also available using a hashtag `#`. The hashtag and everything that follows, before the next newline character `'\n'`, is ignored by the compiler.
```nml
(embedding) # converts context to 4 vectors
```

### Connecting Ninos
To connect/cascade ninos, write the recipient nino's name on a line following the sender nino.
```nml
(embedding) # converts context to 4 vectors
attention1
```
Sometimes you want the output of a nino to be split and handled by multiple different ninos. This can be done by listing each nino with a comma separating them.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedfoward4 # process each vector individually
```
A collection of parallel ninos, like above, can also be converged into a smaller number of ninos.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedfoward4 # process each vector individually

attention_half, attention_half
```

### Reusing Ninos
A neural nino can be reused in two ways. Either, a brand new copy is created which has its own parameters, or each instance shares parameters. By default, using a nino's name will create a new copy with its own parameters.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

attention_half, attention_half
feedforward, feedforward, feedforward, feedforward
```
To create an instance that shares global parameters with other instances, add an equal sign `=` to the start of the name. This doesn't affect functional ninos.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedfoward4 # process each vector individually

attention_half, attention_half
=feedforward, =feedforward, =feedforward, =feedforward
```
Other times, you'll want the output of a single nino instance to be duplicated. This can be done by surrounding the name in curly brackets `'{'` `'}'` , and adding a non-negative multiplier.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedfoward4 # process each vector individually

attention_half, attention_half
{=feedforward; 2}, {=feedforward; 2}, =feedforward, =feedforward # outputs 2+2+1+1 vectors
```
Rewriting the name of a nino can become tedious. A macro `[name; n]` is available that duplicates a name, including when `=` or `{}` is used.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[{=feedforward; 2}; 2], [=feedforward; 2] # outputs 2*2 +2 vectors
```

### Forwarding Vectors
Sometimes you want to ignore some vectors for a stage, but need them in the next stage. This can be done by using the greater-than sign `>`.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[{=feedforward; 2}; 2], [=feedforward; 2] # outputs 2*2 +2 vectors

attention_half, >
```
You may not want the compiler to infer how many vectors should be forwarded to the next stage. This can be specified by adding a non-negative integer before the `>`. This is especially important because the compiler can only infer once per stage.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[{=feedforward; 2}; 2], [=feedforward; 2] # outputs 2*2 +2 vectors

1>, attention_half, > # infers that 3 vectors are forwarded on the right
```
Forwarding can also rearrange vectors, by specifying a non-negative integer after the `>`. This value becomes the index of the first vector being forwarded. Index 0 represents the first vector in the following stage. The code `3>5` forwards three vectors to become the 6th, 7th and 8th vectors. All remaining vectors that weren't forwarded to a specific index will then fill the gaps, in their original order.<br>
An error will be thrown if groups of vectors are forwarded to colliding places, such as in the case of `3>0, 2>2`, where there is a collision at the 3rd (index 2) vector. An error will also be thrown if there are not enough remaining vectors to fill all gaps.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[{=feedforward; 2}; 2], [=feedforward; 2] # outputs 2*2 +2 vectors

1>, attention_half, > # infers that 3 vectors are forwarded on the right
1>1, 1>0, > # swaps the 1st and 2nd vectors
```

### Dropping Vectors
You may instead want to completely drop some vectors, because they're not needed for the next stage. This can be done with an exclamation mark `!`.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[=feedforward; 4]

1>, attention_half, > # infers that 3 vectors are forwarded on the right
1>1, 1>0, > # swaps the 1st and 2nd vectors
=feedforward, !, [=feedforward; 2] # drop the 2nd, 3rd and 4th vectors
# output here is 3 vectors, with no gaps
```
Just like with forwarding, you can specify the number of vectors by adding a non-negative integer before the `!`. Remember that the compiler can only infer once per stage, regardless of if it's `>` or `!`.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[=feedforward; 4]

1>, attention_half, > # infers that 3 vectors are forwarded on the right
1>1, 1>0, > # swaps the 1st and 2nd vectors
=feedforward, 2!, [=feedforward; 2], ! # drop the 2nd, 3rd and 6th vectors
# output here is 3 vectors, with no gaps
```

### Zero Vectors
NML supports introducing zero-vectors midway through the pipeline, as a quick way to default some inputs to zero. This is done by adding a non-negative number followed by a plus sign `+`.
```nml
(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[=feedforward; 4]

1>, attention_half, > # infers that 3 vectors are forwarded on the right
1>1, 1>0, > # swaps the 1st and 2nd vectors
=feedforward, 2!, [=feedforward; 2], ! # drop the 2nd, 3rd and 6th vectors
# output here is 3 vectors, with no gaps

>, 1+
unembedding # receives 4 vectors
```
For more advanced ways of introducing vectors, the most versatile method is by using a functional nino.

### Macros
Constant macros can be defined using the backlash `\`, followed by a valid name, then followed by a colon `:`, and lastly followed by the text/value that will replace this macro.
```nml
\VECS: 4

(embedding) # converts context to 4 vectors
attention1
feedforward1, feedforward2, feedforward3. feedforward4

[attention_half; 2]
[=feedforward; VECS]

1>, attention_half, > # infers that 3 vectors are forwarded on the right
1>1, 1>0, > # swaps the 1st and 2nd vectors
=feedforward, 2!, [=feedforward; 2], ! # drop the 2nd, 3rd and 6th vectors
# output here is 3 vectors, with no gaps

>, 1+
unembedding # receives 4 vectors
```
All text matching the macro name will be replaced before any other processing happens.

## File Tree
TODO: Describe the file tree required for NML compiler to find relevant ninos.
