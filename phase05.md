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
