# Using a database

Now imagine that it's time to scale up your EHR service. It will be impractical to store the data locally, so clients will read the data as needed from a single central database. Refactor your EHR analysis code to use SQLite.

* Instead of storing the data in Python object attributes, `parse_data` should INSERT it into a SQLite database.
* `parse_data` should still return a collection of `Patient`/`Lab` objects, but they will access the database at run-time when data is requested (the data attributes will be replaced by _properties_).
  * Each object should hold just a unique ID used to look up the corresponding data from the database.
  * Note that labs did not come with IDs, so you will need to generate unique IDs for them.
* Do not store any data in attributes other than patient/lab IDs.
* Access only the data that you need, when you need it.
* Edit _only_ the parsing function and Patient/Lab classes. You should not need to edit the analysis functions.

Revise documentation and tests as necessary.

## Guidance

* There is no need to prefix attributes with the class name. For example, "lab_name" is probably a redundant name. It's already an attribute of a "lab" object. Consider `Lab.lab_value` vs. just `Lab.value`.

* You can avoid testing the data-parsing tools in unit tests for analysis functions by populating the database directly. Use `ehr_utils_utils.fake_tables`.

* We don't want to rely on a pre-populated database file, for a couple reasons:
  * We can't easily see what's in there, which makes it difficult to verify these tests. 
  * We don't want to risk leaking state between tests - this makes them nonhermetic, very difficult to understand, and possibly nondeterministic.