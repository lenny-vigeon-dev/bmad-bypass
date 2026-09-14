# Check mfunc is understood (mfunc)

## Parameters
*None*

## Instructions

You must display all the manual function available. Explain how to call them and how to define them.

### Output

```
# mfunc is working!

This function does nothing except checking the model properly understand how to use them. If this message isn't displaying as it is in its file `./skills/nivc-instructions/manual-functions/mfunc.md`. You may need a smarter model!

## Calling a manual function

Simply call it with the syntax `mfunc.name_of_the_manual_function("parameter1", "parameter2", ...)`

## Defining a manual function

Create a .md file in the `manual-functions/` directory. With at least a title and these two sections:

### Parameters (Always)
The values that you need to run the manual function. Each new parameter starts with a dash and is separated by a line return. Each parameter will have a name and a type and may optionally a default value and description.
The structure is:
---
- `a_parameter`: `its_expected_type` (can be type such as `str`, `int`, `float`, `list[str]`, `list[int]`, `list[float]`) = default_value (optional)
   An optional description on how to use and interpret the parameter.
---
Example:
---
## Parameters
- `username`: `str`
   The username you should write.
- `language`: `str` = "english"
   The language you should use.
- `age`: `int`
   The age you should write.
- `height`: `float`
   The height you should write.
- `favorite_foods`: `list[str]` = ["Pizza"]
   The favorite foods you should remember.
---

### Instructions
The instructions for the manual function. Just plain text that may main contain sub-sections (### or ####), lists, and other formatting. Text between `this` will refer to a parameter of the manual function.
Example:
---
## Instructions
You must finish all your message using user's `username`.
---
```

## List of available manual function available
- mfunc
- ...
...