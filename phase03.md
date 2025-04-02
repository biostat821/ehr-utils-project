# Testing/Documentation

1. Add a suite of unit tests to your EHR library using the `pytest` framework, for all functions. Put your tests in the top-level `tests/` directory. Note that every test file and test function name must start with "test_". 

_Do not commit the data files._ You can add _small_ fake data files for testing (in tests/), or use `fake_files` from [`ehr_utils_utils`](https://github.com/biostat821/ehr-utils-utils).

* To write _unit_ tests, we want to call only one of your functions in each test. In particular, that means that when testing your analysis functions, you should generate the input directly rather than calling `parse_data` to do it.
* Code coverage is not everything. You should also attempt to test logical paths. For example, if all of your tests for `patient_is_sick` assert that the result is `True`, your tests would also pass for the following implementation:
```python
def patient_is_sick(records: DATA_TYPE, patient_id: str, lab_name: str, operator: str, value: float) -> bool:
    """Determine whether a patient is sick."""
    return True
```

2. Edit the top-level `README.md` file to include documentation for both user classes: end users and contributors.

    Include for end users:

    * setup/installation instructions, including information about the expected input file formats
    * examples

    Additionally, include for contributors:

    * local testing instructions

    Make sure to look at your file through a Markdown rendered; both VSCode and GitHub have one built in. If you're new to Markdown, [learn it](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).
