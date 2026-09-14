---
name: bbps-guess-tests
description: "Find tests to implement based on a Git diff."
---

> Make sure to load the skill /bbps-instructions

## Overview

This skill examines the diff between the current commit (HEAD) and a commit you specify (the user may also ask you to guess on uncommited/unstaged changes instead), then guesses what tests are needed for each new/modified feature.

## On activation

- 0. Look for any other available skills that could help make tests
  - **If you can't find skills to make tests**
    - Jump step 1
  - **If you find skills to make tests**
    - call mfunc.muchpi(
        "I found some skills that may be useful for making test, what skill would you like me to use?",
        "The first answer option should be this skill with this title: 'Use /bbps-guess-tests (the current loaded skill)', followed by as many options as skills you found with the format: 'Use /this-other-skill-you-found"
      )

- 1. Read the git diff
  - **If the user hasn't specified a commit to diff against**, ask them now (Unless the diff is about uncommited/unstaged changes). Call mfunc.git_diff_request()
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
