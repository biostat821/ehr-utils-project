# Initial implementation

Develop a Python module that provides some simple analytical capabilities on some (synthetic) EHR data. This is provided as:

* A table of patients with demographic data: `PatientCorePopulatedTable.txt`: http://people.ee.duke.edu/~pkw3/static/PatientCorePopulatedTable.txt
* A table of laboratory results: `LabsCorePopulatedTable.txt`: http://people.ee.duke.edu/~pkw3/static/LabsCorePopulatedTable.txt

## Data parsing

Define a function `parse_data(patient_filename: str, lab_filename: str) -> DATA_TYPE` that reads and parses the data files. 

## Analysis

Define the following functions to interrogate the data.

### Old patients

The function `patient_age(records: DATA_TYPE, patient_id: str, age: float) -> int` should take the data and return the age in years of the given patient. For example,

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

All of this should be generalizable, i.e. it should be designed to work with files with these formats, not just these _specific_ files. State (in module/function docstrings) any assumptions that you make about the input data.

You may like to use the `datetime` (standard) library. _Do not import any other libraries._

Your submission should pass checks by `black`, `mypy`, `pycodestyle`, and `pydocstyle`. You can see the specifics of these checks in the repository's GitHub workflow specification: `.github/workflows/checks.yml`.
