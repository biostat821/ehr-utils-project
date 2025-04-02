# Using a database

Now imagine that it's time to scale up your EHR service. It will be impractical to store the data locally, so clients will read the data as needed from a single central database. Refactor your EHR analysis code to use SQLite.

* Instead of storing the data in Python object attributes, `parse_data` should INSERT it into a SQLite database.
* You will still have Patient and Lab classes, but they will access the database at run-time when data is requested (the data attributes will be replaced by _properties_).
  * Note that in order to look up individual labs from the database, you will need to generate unique IDs for them.
* Do not store any data in attributes other than patient/lab IDs.
* Access only the data that you need, when you need it.
* Edit _only_ the parsing function and Patient/Lab classes. You should not need to edit the analysis functions.

Revise documentation and tests as necessary.
