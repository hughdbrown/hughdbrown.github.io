---
title: "Fast Pandas Transform"
date: 2023-07-04T11:49:31-06:00
draft: false
---
At a recent tech meetup, there was a question on a code bottleneck. A data scientist had a zip code on each row and needed to apply a transform to attach data that was provided as a range of zip codes. You can think of it like this:

```
CREATE TABLE ZipCodeMapping (
  LowerZipCode INT,
  UpperZipCode INT,
  StateCode VARCHAR(5)
);

INSERT INTO ZipCodeMapping (LowerZipCode, UpperZipCode, StateCode)
VALUES
    (1001, 2791, 'MA'),	
    (2801, 2940, 'RI'),
    (3031, 3897, 'NH'),
    (3901, 4992, 'ME'),
    ...
;

SELECT StateCode
FROM ZipCodeMapping
WHERE 12345 BETWEEN LowerZipCode AND UpperZipCode;
```

What's a fast way to do this transform with python / pandas / jupyter notebook (i.e. a current data science platform)?

The obvious solution is to do a serial lookup into the range for each row. The problem is that this is too slow: it is proportional to the number of rows in the lookup.

Another way would be to do a binary search on the lookup table, finding the zip code range that a zip code falls into in O(log n) time. Faster, but not the best we can do.

The fastest way I could think of was to convert the sparse representation of `(LowerZipCode, UpperZipCode)` to a dense representation. Then we can put all of the zip codes into the lookup table and do this really quickly.

When the data scientist implemented it at work, her time for a batch operation on 1,000,000 rows went down from 19.5 seconds to 0.3 seconds, taking 1/65 of the previous time.

The relevant bit of magic was the use of the pandas API methods [DataFrame.assign](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.assign.html) and [Series.map](https://pandas.pydata.org/docs/reference/api/pandas.Series.map.html):
```
df.assign(value=df.zip.map(lookup))
```
This creates a new column named `value` on the DataFrame `df`, with the column `zip` as the source key for searching into the dictionary `lookup`.

[Code](https://gist.github.com/hughdbrown/2f125de201ef70e75c8ca6af95834e8b)
[Results](https://gist.github.com/hughdbrown/5d16b27721ae4a92fbac3b1733ab68a7)
