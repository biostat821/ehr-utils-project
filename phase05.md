# Object-oriented EHR utils

Refactor your EHR analysis code to use an object-oriented approach.

Replace your `dict`-based data structures with custom classes.

At minimum you should end up with:

* a `Patient` class with:
  * instance attributes for gender, DOB, race, etc.
* a `Lab` class with:
  * instance attributes for value, units, etc.

Delegate the implementation of your `patient_age` function to a new `Patient` [property](https://docs.python.org/3/library/functions.html#property) called `age`. The `patient_age` function should still exist, but most of its logic should move into the `age` property.

Delegate the implementation your `patient_is_sick` function to a new `Patient` method called `is_sick`. The `patient_is_sick` function should still exist, but most of its logic should move into the `is_sick` method.

What should you do with your earliest-admission function?

Update your tests and documentation as necessary.

**TIP:** Try to change your tests as little as possible. You should only need to change the object that you expect from `parse_data` and the object that you pass into the other methods.

## Common guidance

1.  I recommend doing this in two steps. First, replace your `dict`-based data structures with simple custom classes that just hold data and do nothing else.

    ```python
    foo = {
        "a": 1,
        "b": "b",
        "c": true,
    }
    ```

    becomes

    ```python
    class Foo:
        def __init__(self, a: int, b: str, c: bool):
            self.a = a
            self.b = b
            self.c = c

    foo = Foo(1, "b", true)
    ```

    This provides static, typed attributes, eliminating the need for `Union`s and type assertions and key presence checks. It also makes interacting with the objects easier, e.g. `foo.a` rather than `foo["a"]`.

    Make sure everything works as before with this new data container before moving on. Then start moving the analysis functionality into the new objects.

1.  Use appropriate types for class attributes. Your `Patient` class needs a date of birth as a `datetime` object. The date of birth is provided in the TSV file as a string. Where should you convert it? We should be driven by the philosophy of separation of concerns. The `Patient` object's responsibility is to hold a single patient's data and provide some analysis utilities around it; the `parse_data` function's responsibility is to generate `Patient`s from TSV files. So in this case, the string parsing should happen in `parse_data`, and each `Patient` should be initialized by passing it a pre-parsed `datetime` object. Similarly for lab dates and lab values.

1.  The form of your `parse_data` and analysis functions should not change. The `DATA_TYPE` passed between them will, but that's all.

1.  Define `__eq__` for custom classes to make test assertions easier.