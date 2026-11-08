# neurino - documentation index
## For Advanced Users


## For Developers
### File tree
```
┣━ defaults/ # default ninos
┣━ docs/ # extensive documentation
┃  ┣━ details/ # documentation on how the project works
┃  ┃  ┗━ NML.md # explanation of how to use Neurino Markup Language
┃  ┣━ BENCHMARKS.md # performance comparisons
┃  ┣━ CHANGELOG.md # changes per version, since stable release
┃  ┣━ INDEX.md # this file - starting point for the documentation
┃  ┗━ TODO.md # acknowledged issues and planned features
┣━ env/ # details about the development environment
┃  ┗━ requirements.txt # the Python dependencies
┗━ src/ # source code
```

### Contributing
See [CONTRIBUTING.md](../CONTRIBUTING.md)


## Rust
Why not rewrite it in Rust? :crab:<br>
We considered that, and one of the earlier versions of this project was even being built up in Rust. we opted for C because we require very fine optimisations. Implementing this in Rust would generally require a lot of `unsafe` usage, or very verbose code.
