# bmad-bypass
> Looks like your work using bmad, don't use it !

Force to use BMAD in your project but don't want to?

**I got you!**

This repo aims to progressively provide a set of skills to work without [bmad-method](https://github.com/bmad-code-org/bmad-method) while staying within the bmad ecosystem to ensure it doesn't break the bmad workflow for the one using it in your project.

The main goal of these skills is not to go against bmad but to offer an alternative approach that is bmad compatible, letting you work along bmad power users while not being forced to use bmad yourself.

# Skills

## bmad-guess-story

`bmad-guess-story` reconstructs a BMAD-compatible story from an existing Git diff.

Use it when code has already been implemented outside the normal BMAD flow, but your project still needs a story with acceptance criteria, completed tasks, file changes, and a changelog.

Instead of following:

```txt
bmad-create-story -> bmad-dev-story
```
it lets you work backward:
```
write your code/existing code -> inferred story -> bmad-create-story
```