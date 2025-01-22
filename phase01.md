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

You may like to use the following (standard) libraries.
* `datetime`
* `typing`
* `operator`

_Do not import any libraries other than these._

Do not commit the data files. They are not a part of your library.

Put your Python files/module in the src/ directory.

Your submission should pass checks by `ruff` and `mypy`. You can see the specifics of these checks in the repository's GitHub workflow specification: `.github/workflows/checks.yml`.


## General guidance

* Do not name your files "hw1.py" or even "phase01.py". We will be updating these files at every phase rather than creating new ones, so something informative like "ehr_utils.py" would be more appropriate.
