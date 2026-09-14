---
name: bbps-guess-story
description: "Reconstructs a BMAD-compatible user story from an existing Git diff."
---

> Make sure to load the skill /bbps-instructions

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

If bmad mention something that isn't mentioned here but said as mandatory, you must call mfunc.muchpi(
  "Shall we do this thing... / bmad-method expects... (adjust the question based on what you need to ask)",
  [
    "Yes/Do that first (Or any suitable answer in the context)"
    "No/Ignore it (Or any suitable answer in the context)"
  ]
)
If the user decides to ignore it, continue.

## Principles
- Read all the files shown by the git diff.
- Never delete/create a file, unless explicitly request by the user.
- Always ask for the commit to compare with. (Unless provided to you at the skill call)
- Ensure to speak and write the story in the language of {communication_language}`

## On Activation

### 0 Initialization

You MUST not edit any file during this step

- 0. Verify bmad skillset exists
  - Check that at least `/bmad-create-story` is available
  - If not, stop and tell the user to install bmad from: https://github.com/bmad-code-org/bmad-method

- 1. Load project context
  - Search for `**/project-context.md`
    - If found, load as foundational reference for project standards
    - If not found, continue without it

- 2. Read the git diff
  - **If the user hasn't specified a commit to diff against**, ask them now (Unless the diff is about uncommited/unstaged changes). Call mfunc.git_diff_request()
    Stay in this step until they provide something concrete (e.g., "compare with develop HEAD", "from commit a1b2c34d", "from 5 commits ago")
  - Once you have a target commit, read **ALL** files that appear in the diff between current HEAD and that commit

- 3. Check for test
  - If present, continue to step 4
  - If not present, you must `show a multiple-choice picker` with the question "BMAD often expect test for every new feature, shall we add some tests before guessing the story?" "Yes" "No"
    - If yes load and follow the skill `/bbps-guess-tests` and get back to this step once completed.
    - If no, continue.

Continue to main step 1 The Guess

### 1 The Guess

You MUST not edit any file during this step

- 1. Guess the story
    - Ask clarifying questions about any code whose purpose is unclear or ambiguous
    - Document in your context:
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
        - Retrospective (if possible):
          - If you have a context about the feature implementation, write down here all the user feedback (if they said you implemented something wrong, did something you shouldn't) The main idea is that what are the feedback you can do to yourself to avoid the user to correct you and predict what they expect from you.

- 2. Guess the epic/story-number
  - Look into the already existing stories and epics in the project
    - Run find commands looking for keywords such as `*story*.md`, `*epic*.md`, `implementation-artifacts`, `planning-artifacts` and patterns such as `<number>-<number>-*.md` and `<number>.<number>-*.md`.
  - You must add the following text block after writing the story guess in step 4:
    - If you thinks its a new epic you must `show a multiple-choice picker` with the question "It seems this new story doesn't belong to any epic, should I create a new epic?" "Yes" "Yes and I'll give you its name" "No, and I'll tell you which epic it belongs to"
  Epic: `[number-of-the-epic]`-`[epic-title]`
  Story: `[number-of-the-story]` `[story-title]`

- 3. Awaiting authorization / Revision
  - When you completed this first tell the user that you are waiting for their go to run `/bmad-create-story`
    - If you get user approval or they summon `/bmad-crate-story`, follow instructions of main step 2
    - The user may not be satisfied with the story guess, if so ask questions about what they dislike and then repeat the main step 1 The Guess
  - Wait for user input

### 2 Writing down the story (When `/bmad-create-story` skill is called.)

- 0. Updating epic (conditional)
  - If you found a epic file that mentions stories of the same epic you must update the epic file and `/bmad-create-story` may require you to, append the story to the epic as bmad is expecting you to.

- 1. Writing the story
  - Write down the story based on what you wrote in your context and following the `/bmad-create-story` formatting
    - Ensure checking all the tasks as [x].
    - Set status to `review` but ensure using the bmad naming if `review` is not the proper keyword check for the appropriate equivalent status.

- 2. Link the comments to the story
  - In the initial workflow after writing down the story you call `/bmad-dev-story` which implements the feature requested by story as well as writing comments in the code that mention what they implemented belong to story X-X, do the same by reworking or adding comments to what have been implemented to make sure this code is properly refered to the newly crated story.

Once these steps completed inform the user that the story reconstruction is complete and suggest them to call `/bmad-code-review`


