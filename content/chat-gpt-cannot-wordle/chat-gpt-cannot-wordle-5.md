---
title: "Chat Gpt Cannot Wordle 5"
date: 2023-04-21T11:47:42-06:00
description: "Adventures with Chat-GPT, Part 5"
summary: "I give Chat-GPT a third try"
---
I prompted:
> The mask should result in X for a correct letter in the right place, x for a correct letter in the wrong place, and . for a wrong letter.

Chat-GPT apologized:

> I apologize for misunderstanding your earlier question. Here's an updated Rust code that generates the correct mask:

```rust
fn generate_mask(guess: &str, answer: &str) -> String {
    let mut mask = String::new();

    for (i, c) in guess.chars().enumerate() {
        match answer.chars().position(|x| x == c) {
            Some(j) if i == j => mask.push('X'),
            Some(_) => mask.push('x'),
            None => mask.push('.'),
        }
    }

    mask
}
```

> To use this function, you would pass in the guess as the first argument and the correct answer as the second argument. The function will return a String containing a mask for the correct letters in the guess, using "X" to represent correct letters in the correct position, "x" to represent correct letters in the wrong position, and "." to represent incorrect letters.
> 
> Here's an example of how you might call the function:

```rust
let guess = "DETER";
let answer = "EERIE";
let mask = generate_mask(guess, answer);
println!("Mask: {}", mask);
```

> In this example, the output would be:

```
Mask: .X.xx
```

> This means that the correct letters in the guess "DETER" are E and E, which are in the 2nd and 4th positions, respectively, in the correct answer "EERIE". The letter R in the guess is also correct but in the wrong position, hence represented by "x".

[Still not right.](/chat-gpt-cannot-wordle/chat-gpt-cannot-wordle-6)
