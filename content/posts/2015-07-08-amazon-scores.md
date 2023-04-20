---
categories:
date: 2015-07-08 
description: What if Amazon had different scoring for pages?
slug: amazon-scores
tags:
title: Amazon scores
---
It has kind of bugged me for a while that Amazon uses a weighted average
to score products. Really, it’s not how I think of it. The closest thing I
have used is to look at how many 1-star ratings there are versus the 5-star
ratings, and to divide the 5-star count by the 1-star count. A product that
is 50% 5-star and 50% 1-star is a 1 in my books. I look for at least a 5 --
75% 5-star to 15% 1-star, for example.

The products that I have seen with the best ratings are:

* [Miss Fisher Murder Mysteries 1](http://www.amazon.com/Miss-Fishers-Murder-Mysteries-1)
* [Miss Fisher Murder Mysteries 2](http://www.amazon.com/Miss-Fishers-Murder-Mysteries-2)
* [I, Claudius](http://www.amazon.com/Claudius-35th-Anniversary-Sian-Phillips/dp/B006JY3OHW)
* [Prime Suspect](http://www.amazon.com/Prime-Suspect-Collection-Helen-Mirren/dp/B0028AEO0M)

As you can see, they are all exceptionally strong.

But I would really prefer if there were a profile option to choose one of several alternative
scoring algorithms. Here are some alternatives I cam up with:

``` python
import exp, log, sqrt

def cube(total):
    return total ** 3

def cuberoot(total):
    return total ** (1 / 3.0)

def square(total):
    return total ** 2

def identity(total):
    return total

def score_param(series, fn1, fn2):
    item_count = sum(series.values())
    total = sum(fn1(score) * count for score, count in series.items())
    return fn2(float(total) / item_count)

def score_weighted(scores):
    return score_param(scores, identity, identity)

def score_sqrt(scores):
    return score_param(scores, sqrt, square)

def score_log(scores):
    return score_param(scores, log, exp)

def score_square(scores):
    return score_param(scores, square, sqrt)

def score_cube(scores):
    return score_param(scores, cube, cuberoot)

def score_cuberoot(scores):
    return score_param(scores, cuberoot, cube)

def score_dict(series):
    return {i: item for i, item in enumerate(series, start=1)}

def main():
    for i in range(11):
        src = {5: 1, 1: i}
        print(
            i,
            score_log(src),
            score_weighted(src),
            score_sqrt(src),
            score_square(src),
            score_cuberoot(src),
            score_cube(src),
        )

if __name__ == '__main__':
    main()
```

The idea is that you apply a function to aggregate the values and then a second function
to scale the aggregated number back onto the range. Like, if you sum the cubed values you
then take the cube root of the result to return to the original range.

My preference is to use `log` and `exp`. I think it does the best job of emphasizing high ratings
and reducing the gain from low ratings.

If you look at the high-rated products I cited, you don't see much change in score:

``` python
import math

vectors = {
    # http://www.amazon.com/Miss-Fishers-Murder-Mysteries-2
    "miss_fisher_1": [0.02, 0.02, 0.05, 0.16, 0.75],

    # http://www.amazon.com/Miss-Fishers-Murder-Mysteries-2
    "miss_fisher_2": [0.0, 0.0, 0.01, 0.06, 0.93],

    # http://www.amazon.com/Prime-Suspect-Collection-Helen-Mirren/dp/B0028AEO0M
    "prime_suspect": [0.01, 0.02, 0.04, 0.11, 0.82],

    # http://www.amazon.com/Claudius-35th-Anniversary-Sian-Phillips/dp/B006JY3OHW
    "i_claudius": [0.02, 0.01, 0.03, 0.06, 0.88],
}

def dot_product(arr, scores):
    return sum(a * s for a, s in zip(arr, scores))

def amazon_score(m):
    return dot_product(m, range(1, len(m) + 1))

def my_score(m):
    values = [math.log(r) for r in range(1, len(m) + 1)]
    return math.exp(dot_product(m, values))

for name, v in vectors.items():
    print("{'-' * 30} {name}")
    print(amazon_score(v))
    print(my_score(v))
```

Results:

```
------------------------------ miss_fisher_1
4.6
4.47129953528
------------------------------ miss_fisher_2
4.92
4.90836573947
------------------------------ i_claudius
4.77
4.66166586061
------------------------------ prime_suspect
4.71
4.61835648708
```

Products that have higher ratios of 1-star ratings are affected more, as shown here:

```
bad = {
    "jaws_the_revenge": [0.31, 0.10, 0.09, 0.11, 0.39],
    "inappropriate_comedy": [0.35, 0.1, 0.14, 0.08, 0.35],
    "staying_alive": [0.10, 0.05, 0.10, 0.11, 0.64],
}

for name, v in bad.items():
    print("{'-' * 30} {name}")
    print(amazon_score(v))
    print(my_score(v))
```

```
------------------------------ jaws_the_revenge
3.17
2.581480288440958
------------------------------ inappropriate_comedy
3.04
2.453038000783671
------------------------------ staying_alive
4.140000000000001
3.7699103886019767
```
