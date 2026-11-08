# neurino - contributing
## Table of Contents
- [Introduction](#introduction)
- [Ways to Contribute](#ways-to-contribute)
- [AI Usage](#ai-usage)
- [Setting up Your Environment](#setting-up-your-environment)
- [Styling Guide](#styling-guide)

---

## Introduction
Hi! If you're reading this, then you're possibly interested in contributing. If you do, then thank you for your time and energy building up this project!

---

## Ways to Contribute
You're always welcomed to suggest your own features/improvements, but consider looking at [TODO.md](docs/TODO.md). This is a list of every feature/improvement that has already been approved and we are hoping to complete. You can hunt down the relevant contributor for a task that's marked as "In progress", but it's usually easier to pick up a task marked "Not started".

---

## AI Usage
We stand neutral with regards to the use of artificial intelligence as a programming tool. We neither encourage nor discourage it.

---

## Setting up Your Environment
### Requirements
| Tool | Version |
| - | - |
| :octocat: Git | >= 2.56.0 |
| :snake: Python | >= 3.14.8 |
| :robot: C | >= C23 |

### Recommendations
| Tool | Version |
| - | - |
| :wrench: GNU Make | >= 4.4.1 |

### Steps
These steps assume you have all the necessary tools installed.

1. **Fork**

Fork the repository on GitHub. This creates your own copy of the repository, which you will later request to merge with the main copy.

2. **Clone**

Clone the fork of the repository to your local device. If you're using the Git CLI, we recommend using the following command:
```bash
git clone --recursive <url-of-your-forked-repository>
```
> [!TIP]
> If you're unfamiliar with this process, open the folder where you want to clone the repository, using your file explorer. This is typically a `Projects` folder, with all your other projects. Right-click and select to open a terminal. Retype the above command into the terminal and press Enter. If you receive issues about access/credentials, consider making your fork public, or searching for help on the internet (such as YouTube). Once this is done, run the command below to prepare for the next step.
```bash
cd <name-of-your-forked-repository>
```

3. **Virtual Environment**

For Python, we recommend setting up a virtual environment. Use the name `.venv`, because it's what the `.gitignore` is looking for. Below are examples of the commands needed for the default OS-shell pairings.

<details>
<summary>Click to view commands for Windows Powershell</summary>

:window:<br>
Create a virtual environment using:
```powershell
python -m venv .venv
```
and activate it using:
```powershell
.venv\Scripts\Activate.ps1
```

</details>

<details>
<summary>Click to view commands for MacOS Zsh</summary>

:apple:<br>
Create a virtual environment using:
```zsh
python3 -m venv .venv
```
and activate it using:
```zsh
source .venv/bin/activate
```

</details>

<details>
<summary>Click to view commands for Linux Bash</summary>

:penguin:<br>
On a Debian based distro (such as Ubuntu) you may need to install the virtual environment tool:
```bash
sudo apt update && sudo apt install python3-venv
```
Create a virtual environment using:
```bash
python3 -m venv .venv
```
and activate it using:
```bash
source .venv/bin/activate
```

</details>


4. **Dependencies**

Now that you have an activated virtual environment, you can install the Python dependencies using:
```
pip install -r env/requirements.txt
```

5. **Contribute**
Make some changes

---

## Styling Guide
The styling guide is negotiable at the moment, while the project is still small.

### C Formatting
The requirements are:
- **file names**: File names must be in lowercase snake case, except where there's a strong convention for a different naming scheme (such as `CONTRIBUTING.md`).
- **title**: Every file must have a title, which briefly describes the purpose of that file. This can be duplicated for header and implementation files, so long as their is some distinction.
- **date**: Every file must show the date of when it was last updated, in terms of any functional (non-comment) changes. Changes to comments don't have to reflect on the date.
- **no author**: Please do not add your name as an author to the file. This project is a team effort, and your contribution is recorded on GitHub.
- **function docs**: Every function must be documented with a comment block above it, describing the purpose (`@brief`), parameters (`@param`) and return value (`@return`).
- **camel case**: Variables and functions must be named using camel case. Constants and macros are completely uppercase, so they receive snake case.
- **indentation**: Indentation uses 4 spaces.

The recommendations are:
- **notes**: Functions should be documented with notes (`@note`), where the implementation has some non-obvious implications to the user.
- **file docs**: Files should have a comment block at the top, describing the purpose of the file in detail.
- **file length**: No file should be 1000 lines or longer. 999 lines is the maximum.
- **fancy code**: Add a block comment for explanation, where a section of code is doing something fancy.
- **sectioning**: Separate large sections of code, where appropriate, with a title in uppercase letters, and 32 `=` characters afterwards.
- **general**: If there's uncertainty about anything not listed, look at an already existing implementation as a guide.
```c
/*
    Module to get the sign of integers
    Last updated: 1 January 2000
*/

// API FUNCTIONS ================================

/**
 * @brief Gets the sign of an integer
 * 
 * @param num The integer to get the sign of
 * 
 * @return The sign of the integer, as either `1` or `-1`
 * 
 * @note This function doesn't follow the typical signum definition. When receiving 0, this function returns 1.
 * @note This is done because a value of 1 is more useful in vector algebra, which this module is used for
 */
int signum(int num) {
    return (num >= 0) ? 1 : -1;
}

```

### Python formatting
The requirements are:
- **file names**: File names must be in lowercase snake case, except where there's a strong convention for a different naming scheme (such as `CONTRIBUTING.md`).
- **title**: Every file must have a title, which briefly describes the purpose of that file.
- **date**: Every file must show the date of when it was last updated, in terms of any functional (non-comment) changes. Changes to comments don't have to reflect on the date.
- **no author**: Please do not add your name as an author to the file. This project is a team effort, and your contribution is recorded on GitHub.
- **function docs**: Every function must be documented with a docstring under the header, describing the purpose, parameters (`Args:`), return value (`Returns:`), and exceptions (`Raises:`).
- **explicit typing**: Every function's parameter and return types must be stated where possible.
- **snake case**: Variables, functions, constants, et cetera, must be named using snake case.
- **indentation**: Indentation uses 4 spaces.<br>
The recommendations are:
- **file docs**: Files should have a comment block at the top, describing the purpose of the file in detail.
- **file length**: No file should be 1000 lines or longer. 999 lines is the maximum.
- **fancy code**: Add a block comment for explanation, where a section of code is doing something fancy.
- **sectioning**: Separate large sections of code, where appropriate, with a title in uppercase letters, and 32 `=` characters afterwards.
- **general**: If there's uncertainty about anything not listed, look at an already existing implementation as a guide.
```python
"""
    Module to get the sign of integers
    Last updated: 1 January 2000
"""

# API FUNCTIONS ================================

def signum(num:int) -> int:
    """
    Gets the sign of an integer.
    This function doesn't follow the typical signum definition. When receiving 0, this function returns 1.
    This is done because a value of 1 is more useful in vector algebra, which this module is used for
    Args:
        num (int): The integer to get the sign of
    Returns:
        int: The sign of the integer, as either `1` or `-1`
    """
    if num>=0:
        return 1
    else:
        return -1

```
