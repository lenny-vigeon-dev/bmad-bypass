---
name: bbps-code-review
description: "Overloads /bmad-code-review."
---

> Make sure to load the skill /bbps-instructions

## Overview

Run /bmad-code-review and do what the skill intruct you to do. Once the review complete and when it comes to decides what to do with all the `decision`, `patch`, `defer` and `dissmiss` override the initial behaviour to format the issues in a specific way.

## On Activation

### 0 Initialization

- 0. Verify bmad skillset exists
  - Check that at least `/bmad-code-review` is available
  - If not, stop and tell the user to install bmad from: https://github.com/bmad-code-org/bmad-method

- 1. Run the skill normally
  - Read and run the skill `/bmad-code-review` run all the steps up to when you write in the story the review finding after that this skill takes over the normal course of `/bmad-code-review` and you must go to section 1 (The override)

### 1 The override

- 1. The decision(s)
  - If no `decision` tier review finding have been found
    - Jump to step 2
  - If at least one `decision` tier review finding have been found
    Loop the following instruction over each `decision` that have been found:
    - a. Explain the issue found in less than 5 sentences, stay concise
    - b. Explain why this is an issue in less than 5 sentences, stay concise
    - c. Showcase the fix options in less than 5 sentences each, stay concise
    - d. Let the user pick a decision by calling mfunc.muchpi(
        "How should we solve this issue?",
        "List first your recommended solution followed by all you other solutions, followed by 'Defer (Moves the issue as deferred work)' then 'Ignore and discard (Leaves the issue unsolved and removes it from the review finding in the story)' finally 'Something else (Let the user explain their idea on how to fix the issue or ask clarifications or anything else)'"
      )
      - If the user picks `Something else` or give a custom answer
        Answer their sub request and redo sub-step `d.`
      - If the user picks any other options
        Apply fix, and update story
    Example of how to display:
    ```
    **The issue**: Agents found a high complexity O(n^5) loop that could highly deteriorate performance at scale.
    **Why is it an issue**: With only 10 iterations for each level of loop it would bring you to 100 000 iteration that could freeze the code for few seconds. An alternative approach with caching could bring down this complexity down to O(n^2) making it more viable.
    **Fix options**
      1. Do a small refactor to optimize performance
      2. Leave as is and accepting the risk
    *call the manual function*
    ```

- 2. The patch(es)
  - If no `patch` tier review finding have been found
    - Jump to step 3
  - If at least one `patch` tier review finding have been found
    Loop the following instructions over each `patch` that have been found:
    - a. Explain the issue found in less than 5 sentences, stay concise
    - b. Explain why this is an issue in less than 5 sentences, stay concise
    - c. Explain how to fix it in less than 5 sentences, stay concise
    - d. Let the user pick a decision by calling mfunc.muchpi(
        "Would you like to apply the fix?",
        [
          "Yes",
          "Defer (Moves the issue as deferred work)",
          "Ignore and discard (Leaves the issue unsolved and removes it from the review finding in the story)",
          "Something else (Let the user explain their idea on how to fix the issue or ask clarifications or anything else)",
        ]
      )
      - If the user picks `Something else` or give a custom answer
        Answer their sub request and redo sub-step `d.`
      - If the user picks any other options
        Apply fix, and update story
    ```
    **The issue**: Agents found a hardcoded API key in the code.
    **Why is it an issue**: API key are often meant to be secret and by sharing them like this you expose yourself abuse usage by ill intentioned hackers. This can lead depending of your key to reach your usage limits or worse make you overspend money for not even your use if the API key is related to a pay as you go service.
    **Fix**: Move the hardcoded API key in the .env file and export it from your environment variables.
    *call the manual function*
    ```

- 3. The deferred
  - If no `defer` tier review finding have been found
    - Jump to step 5
  - If at least one `defer` tier review finding have been found
    Call mfunc.muchpi("Review deffered issues?", ["Yes", "No"])
    - If yes: Continue step 4
    - If no: Jump to step 5

    Loop the following instructions over each `defer` that have been found:
    - a. Explain the issue found in less than 5 sentences, stay concise
    - b. Explain why this is an issue in less than 5 sentences, stay concise
    - c. Explain how to fix it in less than 5 sentences, stay concise
    - d. Let the user pick a decision by calling mfunc.muchpi(
        "Keep deffered?",
        [
          "Yes, keep deferred",
          "No, fix it now",
          "Something else (Let the user explain their idea on how to fix the issue or ask clarifications or anything else)",
        ]
      )
      - Yes, keep deferred
        Proceed and show next `defer` unless it was the last one which then you can jump to step 4
      - No, fix it now
        Apply fix, and update story
      - If the user picks `Something else` or give a custom answer
        Answer their sub request and redo sub-step `d.` if needed
    ```
    **The issue**: Agents found a small breaking change in another feature outside the story scope.
    **Why is it a deferred issue**: The refactor add a new default parameter to the function get_user() that by default behave the same as before, but if its default value were to change it would silently break the output for this other feature. It is however not directly related to this story so you may not want to apply the fix yet.
    **Fix**: Make this optional parameter explicit.
    *call the manual function*
    ```

- 4. The dissmiss(ed)
  - If no `dissmiss` tier review finding have been found
    - Jump to step 5
  - If at least one `dissmiss` tier review finding have been found
    Call mfunc.muchpi("Review dissmissed issues?", ["Yes", "No"])
    - If yes: Continue step 4
    - If no: Jump to step 5

    Loop the following instructions over each `defer` that have been found:
    - a. Explain the issue found in less than 5 sentences, stay concise
    - b. Explain why this is an issue got dissmissed in less than 5 sentences, stay concise
    - c. Let the user pick a decision by calling mfunc.muchpi(
        "Should it be dissmissed?",
        [
          "Yes",
          "No, and I'll give you the fix",
          "No, and suggest me a fix",
          "Something else (Let the user explain their idea on how to fix the issue or ask clarifications or anything else)",
        ]
      )
      - Yes
        Proceed and show next `dissmiss` unless it was the last one which then you can jump to step 5
      - No, and suggest me a fix
        Ask the user to explain the fix they want and once their fix solution given reformulate it and ask the user if you preperly understood their suggestion followed by a call mfunc.muchpi("Confirm apply fix?", ["Yes", "No"])
        - Yes: Apply fix and update story
        - No: Ask the user for precisions, then redo mfunc.muchpi("Confirm apply fix?", ["Yes", "No"]) until the user accept or tell you to give up
      - No, and suggest me a fix
        Explain your fix idea in less than 5 sentences then call mfunc.muchpi("Apply fix?", ["Yes", "No"])
        - Yes: Apply fix and update story
        - No: Ask the user what they want, after sorting out their request and answer proceed and show next `dissmiss` unless it was the last one which then you can jump to step 5
      - If the user picks `Something else` or give a custom answer
        Answer their sub request and redo sub-step `d.` if needed
    ```
    **The supposed issue**: Agents found a that you don't do an assertion for floating value in your function do_integer_add().
    **Why it got dissmissed**: The function force cast the float into an int so there's no scenario where you add a float with and int.
    *call the manual function*
    ```

- 5. Resuming the normal process
 - Once all decisions has been picked for all the issue, resume the instruction of `/bmad-code-review` and conclude the review