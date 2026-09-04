---
name: bbps-guess-tests
description: "Find tests to implement based on a Git diff."
---

## Overview

This skill examines the diff between the current commit (HEAD) and a commit you specify (the user may also ask you to guess on uncommited/unstaged changes instead), then guesses what tests are needed for each new/modified feature.

## On activation

- 1. Read the git diff
  - **If the user hasn't specified a commit to diff against**, ask them now (Unless the diff is about uncommited/unstaged changes). You must `show a multiple-choice picker` with the question "What is the diff comparison point?" "Unstaged/Uncommitted files" "dev/develop bra,ch" "main/master branch" "Default branch" "Something else"
    Stay in this step until they provide something concrete (e.g., "compare with develop HEAD", "from commit a1b2c34d", "from 5 commits ago")
  - Once you have a target commit, read **ALL** files that appear in the diff between current HEAD and that commit

- 2. Suggest tests:
  - Unit tests
  - Functionnal tests (if applicable)
  - End-To-End tests (if applicable)
  - Mock tests (if applicable)
  - Any other kind of tests you can think of that are useful in this case

- 3. Wait for user input
  - If test approved implement them all
  - If not approved by the user ask them what to change/improve
