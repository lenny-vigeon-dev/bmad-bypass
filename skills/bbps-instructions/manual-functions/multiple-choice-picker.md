# Multiple-Choice Picker (muchpi)

## Parameters
- `question`: `str`
   The question to display or an instruction of what kind of question you should write.
- `answer_options`: `list[str] (at least 2) | str`
   Either a finite list of answer options (each answer os either the exact answer option to write or an instruction of what kind of answer you should write) or an instruction of how to generate them.

## Instructions

You must display a multiple-choice picker with the specified question and answer options.

### Output

#### If a multiple-choice picker tool exists
You MUST use it to display the question and all provided answer options. If your tool expect a description for the question/answers, you are free to fill it the way you want but prefer small reformulation to avoid redundancy.

#### If no multiple-choice picker tool exists
Display the `question` and options using the following fallback format:
```
**`question`**

1. `answer_options[0]`
2. `answer_options[1]`
3. `answer_options[2]`
...
```

Tell the user to type the number corresponding to the answer they picked.

### Action

Do what the answer implies. It can be nothing.