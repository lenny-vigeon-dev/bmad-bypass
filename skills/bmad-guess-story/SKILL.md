---
name: bmad-guess-story
description: 'Reverse engineer. Use when the user call it, this skill aims to build story made of Acceptance Criteria, Tasks and list of modified files. It should followed up with the skill /bmad-create-story.'
---

## Overview


This skill acts as a Reverse Engineer that examines the diff between the current commit (HEAD) and a commit you specify, then guesses what feature(s) these changes represent. You must document in your context:
- Acceptance Criteria (ACs)
- Use cases
- Tasks that were likely completed
- Files that have been modified

If you're unclear why a file was edited, ask the user for clarification. Once you have full visibility, ask the user to run the `/bmad-create-story` skill.

This skill helps recreate a coherent story retroactively, as if the bmad workflow had been followed from the start. It does the same job as running the skills `/bmad-create-story` followed with `/bmad-dev-story`.

This skill provides a Reverse Engineer who looks into the diff between the current/active commit and the commit requested by the user and do a guess of what kind of feature(s) all the diffs are for. You must write in your context, ACs (Acceptance Criteria), use cases, tasks that were likely completed and the files that have been modified. If you don't know why a file has been edited, you must question the user. Once you have full vision of what these modification are for you must ask the user to run the `/bmad-create-story` skill or propose them to run it for them. This skill aim to fool bmad skillset into believing the user created a story with `/bmad-create-story` followed with `/bmad-dev-story`, when in reality the user would have developed the features by other means. (e.g Agentic coding, Manual coding)

## Principles

- Read all the files shown by the git diff.
- Never delete/modify/create a file, unless explicitly request by the user.
- Always ask for the commit to compare with.
- Ensure to speak and write the story in the language of {communication_language}`

## On Activation

- 0. Verify bmad skillset exists
    - Check that `/bmad-create-story` and `/bmad-init` are available
    - If not, stop and tell the user to install bmad from: https://github.com/bmad-code-org/bmad-method

- 1. Load config via bmad-init skill
    - Store all returned variables for use throughout
    - Use `{communication_language}` from config for all communications

- 2. Load project context
    - Search for `**/project-context.md`
        - If found, load as foundational reference for project standards
        - If not found, continue without it

- 3. Read the git diff
    - **If the user hasn't specified a commit to diff against**, ask them now. Stay in this step until they provide something concrete (e.g., "compare with develop HEAD", "from commit a1b2c34d", "from 5 commits ago")
    - Once you have a target commit, read **ALL** files that appear in the diff between current HEAD and that commit

- 4. Guess the story
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

- 5. When`/bmad-create-story` skill is called.
    - Ensure checking all the tasks as [x].
    - Set status to `review` but ensure using the bmad naming si check for the appropriate equivalent status.



   **STOP and WAIT for user input** — Do NOT execute menu items automatically. Accept number, menu code, or fuzzy command match.

**CRITICAL Handling:** When user responds with a code, line number or skill, invoke the corresponding skill by its exact registered name from the Capabilities table. DO NOT invent capabilities on the fly.
