# Git diff request (git_diff_request)

## Parameters
*None*

## Instructions

call mfunc.muchpi(
    "What is the diff comparison point?",
    [
        "Unstaged/Uncommitted files",
        "dev/develop branch",
        "main/master branch",
        "Default branch",
        "Something else"
    ]
)

### Action

Depending of what is picked:

#### Unstaged/Uncommitted files
Run `git diff` to show the differences between the working directory and the index.

#### dev/develop branch
Run `git diff dev` (or develop) to show the differences between the current branch and the dev branch.

#### main/master branch
Run `git diff main` (or master) to show the differences between the current branch and the main branch.

#### Default branch
Run `git diff` to show the differences between the current branch and the default branch.

#### Something else
Ask the user to specify the comparison point.