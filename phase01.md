# Initial implementation

Develop a Python module that provides some simple analytical capabilities on some (synthetic) EHR data. The data will be provided as:

* A table of patients with demographic data
  * example: [`PatientCorePopulatedTable.tsv`](http://people.ee.duke.edu/~pkw3/static/PatientCorePopulatedTable.tsv)
* A table of laboratory results
  * example: [`LabsCorePopulatedTable.tsv`](http://people.ee.duke.edu/~pkw3/static/LabsCorePopulatedTable.tsv)

## Data parsing

Define a function `parse_data(patient_filename: str, lab_filename: str) -> DATA_TYPE` that reads and parses the data files.

```python
>> records = parse_data("PatientCorePopulatedTable.tsv", "LabsCorePopulatedTable.tsv")
```

## Analysis

Define the following functions to interrogate the data.

### Old patients

The function `patient_age(records: DATA_TYPE, patient_id: str) -> int` should take the data and return the age in years of the given patient. For example,

```python
>> patient_age(records, "1A8791E3-A61C-455A-8DEE-763EB90C9B2C")
49
```

### Sick patients

The function `patient_is_sick(records: DATA_TYPE, patient_id: str, lab_name: str, operator: str, value: float)` should take the data and return a boolean indicating whether the patient has _ever_ had a test with value above (">") or below ("<") the given level. For example,

```python
>> patient_is_sick(records, "1A8791E3-A61C-455A-8DEE-763EB90C9B2C", "METABOLIC: ALBUMIN", ">", 4.0)
True
```

## Notes

All of this should be generalizable, i.e. it should be designed to work for TSV files with these column names, but the columns may appear in a different order. State (in module/function docstrings) any other assumptions that you make about the input data.

The analysis functions should not read from disk.

You may like to use the following (standard) libraries.
* `datetime`
* `typing`
* `operator`

_Do not import any libraries other than these._

Put your Python files/module in the src/ directory.

Your submission should pass checks by `ruff` and `mypy`. You can see the specifics of these checks in the repository's GitHub workflow specification: `.github/workflows/checks.yml`.


## Common guidance

### Data files

Do not commit the data files. They are not a part of your library.

### File names

Do not name your files "hw1.py" or even "phase01.py". We will be updating these files at every phase rather than creating new ones, so something informative like "ehr_utils.py" would be more appropriate.

### Extra files

Do not include any files that are not part of your library or its tests. Unless they have a clear and unique purpose, they're just clutter.

### Printing

Your library should not print anything unless printing is specifically requested, e.g. with a `print_thing()` method or a `verbose=True` argument.

We're building a programmatic tool that could be called by a human with eyes, or could be called by some other code buried beneath yet more code. If your goal is to provide a result, return a value. If your goal is to report a problem, raise an exception. Debugging print statements should not be in "production" code or automated tests (no one will read them), only in scripts to be run by humans (e.g. in an `if __name__ == "__main__"` block).

### Exceptions

In Python, unexpected states should be communicated via raised exceptions. The following should **not** be used to convey that something has gone wrong:

* A special return value, e.g. `None` or -1. Clients may not check for these and will encounter problems downstream that are harder to debug.
* A print statement. See above.
* A returned `Exception`. Raise the exception instead.

Note that other languages like C++ have different philosophies about errors/exceptions and values representing exceptional cases may be passed from functions, but in Python it's conventional to use exceptions for control flow like this.

### Exception handling

We want to keep `try..except` blocks as small as possible because a) less indentation is better for readability, b) it should be clear to the reader what you're intending to catch (we don't want big catch-alls) and c) this could catch errors you weren't expecting, effectively hiding bugs from you.

### Module-level code

Any code outside of a function definition will run whenever someone imports this file. Things that should only be run when the script is run from the command line should be put in a `if __name__ == "__main__"` block.

You're writing a library. Nothing computationally expensive should happen when a module is imported (reading files, parsing data, etc.), it should happen only when the client chooses (calls a function). In general, almost everything in your module should be inside a function and/or in the `if __name__ == "__main__"` block.

### Column order

You don't need to assume anything about the order of the columns - you should use the column headers to determine which is which.

### Commented-out code

Please remove commented-out code. There is no good purpose it. It typically attempts to serve one of several functions, all of which are better achieved by other means:

* remembering something you did before (git does this better)
* example usage (this should go in separate documentation, like the project README)
* test code (this should go in separate test files)

### Variable names

Variable names in Python should use snake_case: https://peps.python.org/pep-0008/

### Variable names, pt 2

Use informative names: `training_predictors`, not `x` (no credit for saving characters).

### Variable names, pt 3

Use nouns for variables (`thing: int`) and verbs for functions/methods (`do_stuff(thing: int)`).

### Variable names, pt 4

Use plural nouns for collections (`things: list[int]`). It's usually unnecessary to include type information in the name, e.g. avoid `things_list: list[int]`.

### Imports

All imports should go at the top of the file: https://peps.python.org/pep-0008/

### `is_sick` edge cases

For the `patient_is_sick` function, note that there is a semantic difference between "this patient is not sick" and "we cannot determine whether this patient is sick" (for example, if no patient data can be found). The former should return `False`, but the latter should raise an exception.

### Guard clauses

Use "guard clauses" to improve readability.

Don't do this:

```python
if key in my_dict:
    value = my_dict[key]
    for i in range(5)
        value += i
    return value
else:
    raise ValueError("No such key")
```

Instead do this:

```python
if key not in my_dict:
    raise ValueError("No such key")

value = my_dict[key]
for i in range(5)
    value += i
return value
```

This improves readability by reducing indentation and cognitive load for readers.

This also works for `break`ing or `continue`ing out of a loop, etc.

### API consistency

The output from `parse_data`, whatever it is (even if it's a tuple), should be exactly what you pass to the analysis functions.

### Nested `if` blocks

Merge nested if statements - combine multiple nested if statements into one logical expression. This can improve readability and make code easier to maintain. 

### `if..elif` chain vs. map

Consider using maps instead of long if/else chains

Instead of this
```python
if operator == ">":
    return lab_value > value
elif operator == "<":
    return lab_value < value
elif operator == ">=":
    return lab_value >= value
elif operator == "<=":
    return lab_value <= value
```

Use this
```python
return {
    ">": lab_value > value,
    "<": lab_value < value,
    ">=": lab_value >= value,
    "<=": lab_value <= value,
}.get(operator)
```

### Temporary variables

If you make a variable that it used just once, consider inlining it.

Instead of this
```python
thing = calculate_thing()
return thing
```

Use this
```python
return calculate_thing()
```