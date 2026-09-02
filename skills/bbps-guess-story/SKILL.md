---
name: bbps-guess-story
description: "Reconstructs a BMAD-compatible user story from an existing Git diff."
---

## Overview

This skill acts as a Reverse Engineer that examines the diff between the current commit (HEAD) and a commit you specify (the user may also ask you to guess on uncommited/unstaged changes instead), then guesses what feature(s) these changes represent. You must document in your context:
- Acceptance Criteria (ACs)
- Use cases
- Tasks that were likely completed
- Files that have been modified

If you're unclear why a file was edited, why something was implemented, present in a file, ask the user for clarification.

This skill helps recreate a coherent story retroactively, as if the bmad workflow had been followed from the start. The code written in the diff is assumed to be the result of running `/bmad-dev-story`, therefore you are expected to set the finally created story status to `review`.

This skill aim to fool bmad skillset into believing the user created a story with `/bmad-create-story` followed with `/bmad-dev-story`, when in reality the user would have developed the features by other means. (e.g Agentic coding, Manual coding)

## Skill precedence over BMAD `Absolute Instruction`
This skill OVERRIDES the BMAD method.
If an instruction from BMAD conflicts with an instruction from this skill, you MUST follow this skill.
In case of conflict, the instruction from this skill takes precedence over BMAD, BMAD is WRONG.

If bmad mention something that isn't mentioned here but said as mandatory, you must `show a multiple-choice picker` with the question "Shall we do this thing... (adjust the question based on what you need to ask)" and these options "Yes/Do that first" or "No/Ignore it"
If the user decides to ignore it, continue.

## Principles
- Read all the files shown by the git diff.
- Never delete/modify/create a file, unless explicitly request by the user.
- Always ask for the commit to compare with. (Unless provided to you at the skill call)
- Ensure to speak and write the story in the language of {communication_language}`

## On Activation

- 0. Verify bmad skillset exists
    - Check that at least `/bmad-create-story` is available
    - If not, stop and tell the user to install bmad from: https://github.com/bmad-code-org/bmad-method

- 1. Load project context
    - Search for `**/project-context.md`
        - If found, load as foundational reference for project standards
        - If not found, continue without it

- 2. Read the git diff
    - **If the user hasn't specified a commit to diff against**, ask them now (Unless the diff is about uncommited/unstaged changes). You must `show a multiple-choice picker` with the question "What is the diff comparison point?" "Unstaged/Uncommitted files" "dev/develop bra,ch" "main/master branch" "Default branch" "Something else"
    Stay in this step until they provide something concrete (e.g., "compare with develop HEAD", "from commit a1b2c34d", "from 5 commits ago")
    - Once you have a target commit, read **ALL** files that appear in the diff between current HEAD and that commit

- 3. Check for test
    - If present, continue to step 4
    - If not present, you must `show a multiple-choice picker` with the question "BMAD often expect test for every new feature, shall we add some tests before guessing the story?" "Yes" "No"
        - If yes load and follow the skill `/bbps-guess-tests` and get back to this step once completed.
        - If no, continue.

- 4. Guess the story
    - Ask clarifying questions about any code whose purpose is unclear or ambiguous
    - Document in your context:
        - A guess of the epic it belong to (You MUST look into the existing story files that are in ./docs/implementation-artifacts/). Unless after checking ./docs/implementation-artifacts/ folder contradicts it, using the branch name as the story identifier is incorrect.
        - A guess of the Story following the format:
            - **As a** ...
            - **I want to** ...
            - **so** ...
        - A guess of the Acceptance Criteria following the format:
            - **Given** ...
            - **When** ...
            - **Then** ...
            - **And** ...
        - A guess of the tasks that would have been needed to rebuild this exact code where each tasks should follow the format:
            - [x] Task 1: ...
                - [x] Description of subtask 1...
                - [x] Description of subtask 3...
                - [x] Description of subtask 3...
                - [x] ...
        - The file list section that corresponds to the file found in the diff, with following format:
            - ### File List

            - a_folder/a_file.py (MODIFIED — added in deps)
            - another_folder/stuff.ini (NEW)
            - a_folder/src/another_file.py (REMOVED)

        - A change log section with this exact content (always show both the commit hash and its branch):
            - ## Change Log

            - YYYY-MM-DD (today's date): Generated the story based on the diff between commit [current_or_active_commit_hash] of [current_or_active_branch_name] and e5f6a7b8 commit of `branch_name`

- 5. When`/bmad-create-story` skill is called.
    - Ensure checking all the tasks as [x].
    - Set status to `review` but ensure using the bmad naming si check for the appropriate equivalent status.


Once finished establishing the story tell the user what sprint you think this story belong to.
Once the user request/accept you to use the skill `bmad-create-story` make sure to also append the related epic file (if exist) before creating the story.

**STOP and WAIT for user input** — Do NOT execute menu items automatically. Accept number, menu code, or fuzzy command match.

**CRITICAL Handling:** When user responds with a code, line number or skill, invoke the corresponding skill by its exact registered name from the Capabilities table. DO NOT invent capabilities on the fly.
