# Computational complexity/Data structures

<!-- You have a "parsing" function and a couple "analysis" functions. The expected use case looks something like this:

```python
from ehr_utils import parse_data, patient_age, patient_is_sick

records = parse_data("filename.tsv")

# get ages
age_00 = patient_age(records, "patient_id_00")
age_01 = patient_age(records, "patient_id_01")
...
age_99 = patient_age(records, "patient_id_99")

# evaluate sickness
is_sick_00 = patient_is_sick(records, ...)
is_sick_01 = patient_is_sick(records, ...)
...
is_sick_99 = patient_is_sick(records, ...)
``` -->

There are four quantities that affect the time your methods will take:
* `pat_rows` - rows in the patients file
* `pat_cols` - columns in the patients file
* `lab_rows` - rows in the labs file
* `lab_cols` - columns in the labs file

Add a comment for every statement in your code indicating its time complexity as a function of these quantities. Many of them may be O(1), but please state this explicitly.

Describe the overall time complexity of each function, documenting your analysis in the function docstrings. For example, let's say that you want to determine whether each element in the labs file is a decimal number:

> Determining whether a cell is a decimal requires reading the first five characters, which requires 5 `O(1)` operations. This process is performed for each cell, or `pat_rows * pat_cols` times, resulting in `5 * pat_rows * pat_cols` operations. For big-O analysis, we drop the constant factor, yielding `O(pat_rows * pat_cols)` complexity.

After you have provided this analysis in comments/docstrings, open a PR. In a comment on the PR, answer the following:

1. Could your implementation be more efficient? Explain how you would optimize your implementation, perhaps by using more appropriate data structures.

You _may_ then be asked to make the proposed changes before approval.

## Common guidance

1. You should add time complexity for every line. Don't assume that something is fast because it's a single line. For example, the built-in function `sorted()` generally takes $n \log n$ time.

1. Make sure you're editing the existing library file(s) rather than creating new ones.

1. `open(filename)` only opens a file, without reading it, so it takes O(1) time.

1. If you're spending linear time looking up objects based on some unique ID, consider using a dictionary.