# Testing/Documentation

1. Add a suite of unit tests to your EHR library using the `pytest` framework, for all functions. Put your tests in the top-level `tests/` directory. Note that every test file and test function name must start with "test_". 

_Do not commit the data files._

Use `fake_files` from [`ehr_utils_utils`](https://github.com/biostat821/ehr-utils-utils). You will need to add the following to `requirements-test.txt`:
```
ehr_utils_utils @ git+https://github.com/biostat821/ehr-utils-utils.git@main
```

* To write _unit_ tests, we want to call only one of your functions in each test. In particular, that means that when testing your analysis functions, you should generate the input directly rather than calling `parse_data` to do it.
* Code coverage is not everything. You should also attempt to test logical paths. For example, if all of your tests for `patient_is_sick` assert that the result is `True`, your tests would also pass for the following implementation:
```python
def patient_is_sick(records: DATA_TYPE, patient_id: str, lab_name: str, operator: str, value: float) -> bool:
    """Determine whether a patient is sick."""
    return True
```
* If your functions raise exceptions for some cases, use [`pytest.raises`](https://docs.pytest.org/en/stable/reference/reference.html#pytest-raises) to confirm that they raise the expected exceptions in the expected situations.

2. Edit the top-level `README.md` file to include documentation for both user classes: end users and contributors.

    Include for end users:

    * setup/installation instructions, including information about the expected input file formats
    * examples

    Additionally, include for contributors:

    * local testing instructions

    Make sure to look at your file through a Markdown rendered; both VSCode and GitHub have one built in. If you're new to Markdown, [learn it](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

## Guidance

* For `patient_is_sick`, we want to test at least two examples, one that returns `True` and one that returns `False`. Otherwise your function could simply `return True` (or `False`) and pass the tests without doing anything smart at all.

* Instead of `x is False`, use `not x`.

* Rather than making a bunch of small assertions about a collection, consider making one big `==` assertion.

* Look out for superfluous assertions. For example, if you have asserted that `x == 23`, there is no need to assert that `x != 40`.

* Unit tests should attempt to test just one thing. For example, you should not use your `parse_data` to generate inputs in order to test `patient_age` - generate the test inputs explicitly.

* The result of `patient_age` depends on the current time when the function is called. If you don't want your tests to spontaneously break in <=1 year (and you don't), you must trick the tests into using a "current time" that you specify. Use [freezegun](https://github.com/spulec/freezegun).

## Common guidance

1. `is True` is always redundant because the only thing that `is True` is `True` itself. See also https://docs.python.org/3/library/stdtypes.html#truth-value-testing.

1. Instead of `condition is False`, prefer `not condition`.

1. Keep the test data small. It should be easy for a human reading the tests to understand what to expect.

1.  Tests are ideally trivially verifiable.

    This is good:

    ```python
    def test_add_one():
        assert add_one(5) == 6
    ```

    We expect that `add_one()` will add 1, and the test verifies that 5 + 1 = 6.

    This is less good:

    ```python
    def test_mean():
        data = [1, 3, 31, 73, 44]
        total = 0
        count = 5
        for datum in data:
            total += datum
            count += 1

        assert mean(data) == total/count
    ```

    Determining whether this is in fact testing `mean()` correctly involves understanding some non-trivial logic. This test is actually wrong - can you see why?
    
    If our tests need their own tests so that we can trust that they're correct, they're not really improving our overall confidence in things.

    This would be better:

    ```python
    def test_mean():
        assert mean([1, 3, 31, 73, 44]) == 30.4
    ```

    It takes the human reader a little work to figure out that the answer should be 30.4. But its better than re-implementing the `mean()` logic.

1.  Keep tests focused. Unit tests should test just one function.

    ```python
    from my_module import a, b

    def test_a():
        intermediate_thing = a()
        result = b(intermediate_thing)

        assert result == "success"
    ```

    In the above example, both `a` and `b` are tested together; if both contain bugs such that the result still looks good, you cannot tell that `intermediate_thing` is wrong. The following are better unit tests.

    ```python
    from my_module import a, b

    def test_a():
        intermediate_thing = a()

        assert intermediate_thing == "in-progress"

    def test_b():
        result = b("in-progress")

        assert result == "success"
    ```

    Note that other non-unit tests (e.g. "integration" tests or "end-to-end" tests) may be more complex, but these should typically be used _in addition to_ unit tests for the individual components.

1.  Keep tests focused. Setup and assertions should ideally include only the details relevant to the behavior being tested.

    ```python
    def test_rename_student():
        student = create_student(name="Alice", course="BIOSTAT 821")

        rename_student(student, "Bob")

        assert student == create_student(name="Bob", course="BIOSTAT 821")
    ```

    This creates `student` with the `course` attribute, which is unrelated to the behavior of `rename_student`.
    If the irrelevant attribute can be omitted, do that. Often such details are actually required, but should be hidden from readers of the test.

    ```python
    def create_test_student(name: str) -> Student:
        return create_student(name=name, course="test course")

    def test_rename_student():
        student = create_test_student(name="Alice")

        rename_student(student, "Bob")

        assert student.name == "Bob"
    ```

    We use a test helper function `create_test_student`, which uses a default `course`. This way the test does not need to mention `course` at all. In addition, we assert only the specific attribute value of interest, rather than asserting equality of the entire `Student` object

1.  Use the arrange-act-assert structure.

    ```python
    def test_rename_student():
        # arrange
        student = create_test_student(name="Alice")

        # act
        rename_student(student, "Bob")

        # assert
        assert student.name == "Bob"
    ```

    * The "arrange" section constructs things that the test will need, or configures the test environment, e.g. creating file(s).
    * The "act" section calls the function under test.
    * The "assert" section makes assertions about the results of the function - its returned value and/or effects on the test environment.

    It's helpful to include blank lines to demarcate the sections, or even comments as in the example above.

1. Tests should attempt to cover the full scope of _behavior_ exhibited by the function under test. For example, if the function under test returns a boolean, you should test examples that produce both results. If your tests only ever assert that the result is true, they would still pass if your function did simply `return true`.

1. No need to assert on types. `mypy` handles type enforcement.