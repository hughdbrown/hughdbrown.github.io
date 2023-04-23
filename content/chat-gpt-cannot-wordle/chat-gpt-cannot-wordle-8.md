---
title: "Chat Gpt Cannot Wordle 8"
date: 2023-04-21T12:06:09-06:00
description: "Adventures with Chat-GPT, Part 8"
summary: "I ask Chat-GPT to try again"
---
I prompted:
> That code generates "..XXx" for chase and erase. Here are two other incorrect test cases:

```rust
>> generate_mask("deter", "eerie")
".x.xx"
>> generate_mask("chase", "eerie")
"....x"
```

> Here is correct code in python. Generate correct code in rust.

```python
def wordscore(actual, test):
    """
    >>> wordscore('crane', 'toils')
    '.....'
    >>> wordscore('crane', 'slate')
    '..X.X'
    >>> wordscore('crane', 'crete')
    'XX..X'
    >>> wordscore('crane', 'nacre')
    'xxxxX'
    >>> wordscore('tepee', 'beset')
    '.X.Xx'
    >>> wordscore('beset', 'tepee')
    'xX.X.'
    >>> wordscore('erect', 'tepee')
    'xx.x.'
    >>> wordscore('tepee', 'erect')
    'x.x.x'
    """
    correct = Counter(
        p
        for p, t in zip(actual, test)
        if p == t
    )
    incorrect = defaultdict(list)
    for i, (p, t) in enumerate(zip(actual, test)):
        if p != t:
            incorrect[t].append(i)
    d = [
        "X" if p == t else "."
        for p, t in zip(actual, test)
    ]
    removed_poss = Counter(actual) - correct
    for t, n in removed_poss.items():
        for i in incorrect[t][:n]:
            d[i] = 'x'
    return "".join(d)
```

Chat-GPT gave it another try.

> Thank you for providing the corrected code in Python. Here's the Rust implementation based on the same logic:

```rust
use std::collections::{HashMap, HashSet};

fn wordscore(actual: &str, test: &str) -> String {
    let mut correct: HashMap<char, usize> = HashMap::new();
    let mut incorrect: HashMap<char, Vec<usize>> = HashMap::new();
    let mut mask: Vec<char> = vec!['.'; actual.len()];

    for (i, (p, t)) in actual.chars().zip(test.chars()).enumerate() {
        if p == t {
            *correct.entry(p).or_insert(0) += 1;
            mask[i] = 'X';
        } else {
            incorrect.entry(t).or_insert(Vec::new()).push(i);
        }
    }

    for (t, positions) in incorrect.iter().filter(|(t, _)| correct.contains_key(t)) {
        let count = correct.get(t).unwrap() - positions.len();
        for i in positions.iter().take(count) {
            mask[*i] = 'x';
        }
    }

    mask.iter().collect()
}
```

> To use this function, you would pass in the actual word as the first argument and the test word as the second argument. The function will return a String containing a mask for the correct letters in the guess, using "X" to represent correct letters in the correct position, "x" to represent correct letters in the wrong position, and "." to represent incorrect letters.
> 
> Here's an example of how you might call the function:

```rust
let actual = "chase";
let test = "erase";
let mask = wordscore(actual, test);
println!("Mask: {}", mask);
```

> In this example, the output would be:

```
Mask: ..XXX
```

> This means that the correct letters in the test "chase" are E, R, and A, which are in the 1st, 2nd, and 4th positions, respectively, in the actual word "erase". The other letters are incorrect, hence represented by ".".

[I tried this in the evcxr REPL.](/chat-gpt-cannot-wordle/chat-gpt-cannot-wordle-9)
