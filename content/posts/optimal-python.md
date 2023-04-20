---
title: "Optimal Python"
date: 2023-04-20T16:24:18-06:00
draft: false
---

The plan is to summarize the births and deaths by year as a year-over-year change; to produce a cumulative sum on that; and to produce a dict of (year, cumulative-sum) pairs.

The fastest way to produce the summarized births and deaths is to create two `Counters`, one for each series. But the data we have is a series of tuples, so is there a fast way to take apart the tuples?

Yes, we can use the builtin zip to pivot the list of tuples:

```python
>>> a = [(1, 2), (3, 4), (5, 6)]
>>> b, c = zip(*a)
>>> b
(1, 3, 5)
>>> c
(2, 4, 6)
```

Then the births and deaths can be converted into summarized counts:
```python
>>> from collections import Counter
>>> births = Counter(b)
>>> deaths = Counter(c)
>>> births
Counter({1: 1, 3: 1, 5: 1})
>>> deaths
Counter({2: 1, 4: 1, 6: 1})
```

Then we want the differences between the births and deaths by year so we can get deltas:
```python
>>> deltas = births
>>> deltas.subtract(deaths)
>>> deltas
Counter({1: 1, 3: 1, 5: 1, 2: -1, 4: -1, 6: -1})
```

One of the nice things about `Counter.subtract` is that it interpolates a value of 0 when the key is missing on the left.

From here we want to tee up the call to `itertools.accumulate`, and for that we need the deltas copied to a dense array. There are two general ways of doing this: using an ordered array of indexes with `dict.get` or using `sorted()` on the `dict.items()`:
```python
>>> from itertools import accumulate
>>> dense = [deltas[i] for i in range(min(deltas), max(deltas))]
>>> other_dense = [v for _, v in sorted(deltas.items())]
>>> dense
[1, -1, 1, -1, 1]
>>> other_dense
[1, -1, 1, -1, 1, -1]
```

Because sorting is `O(n logn)`, we prefer to use the `range`.

You can see that using the range drops one of the values, but that is preferred by the OP.

Now we are ready to use `itertools.accumulate`:
```python
>>> summed = accumulate(dense)
>>> summed
<itertools.accumulate object at 0x10b23a600>
>>> list(summed)
[1, 0, 1, 0, 1]
```

And we can reconstitute that into a dict in one move:
```python
>>> dict(enumerate(summed, start=min(deltas)))
{1: 1, 2: 0, 3: 1, 4: 0, 5: 1}
```

This works because `dict()` takes an iterator of key-value tuples and `enumerate` generates such a series. We pass in a start value so that we do not get a 0-based series but one that starts with the first birth year.

So here are some things that are helpful to know in the standard library:
- `zip` (especially with the splat-operator)
- `collections.Counter` (which can be constructed from an iterator, unlike `collections.defaultdict(int)`)
- `itertools.accumulate`
- `enumerate` (with its `start` argument) 

Once we are done, we see that we have cut about 40% of the runtime off the naive implementation.
```python
count_living_per_year1(pop): 2.1835102809709497
...
count_living_per_year6(pop): 1.3067941670306027
```

The question then is whether 40% is worth it for code that will require explanation for some python programmers.
