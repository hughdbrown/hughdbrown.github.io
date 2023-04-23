---
title: "Chat Gpt Cannot Wordle 1"
date: 2023-04-21T11:26:05-06:00
description: "Adventures with Chat-GPT, Part 1"
summary: "I ask Chat-GPT to give me rust code to score Wordle"
---
I wrote this prompt:
> Write rust code to score a wordle play against the correct answer.

Chat-GPT responded:

> Sure, here's an example Rust code to score a Wordle play against the correct answer:

```rust
fn score_wordle_play(wordle: &str, answer: &str) -> (usize, usize) {
    let mut correct_chars = 0;
    let mut correct_positions = 0;

    // Count correct characters and positions
    for (i, c) in wordle.chars().enumerate() {
        if c == answer.chars().nth(i).unwrap() {
            correct_positions += 1;
            correct_chars += 1;
        } else if answer.contains(c) {
            correct_chars += 1;
        }
    }

    // Return tuple of correct characters and positions
    (correct_chars, correct_positions)
}
```

> To use this function, you would pass in the Wordle play as the first argument and the correct answer as the second argument. The function will return a tuple of two values: the first value is the number of correct characters (including those in the correct position), and the second value is the number of characters that are both correct and in the correct position.
>
> Here's an example of how you might call the function:

```rust
let wordle_play = "ABCDE";
let correct_answer = "ABCDF";
let (correct_chars, correct_positions) = score_wordle_play(wordle_play, correct_answer);
println!("Correct characters: {}", correct_chars);
println!("Correct positions: {}", correct_positions);
```

> In this example, the output would be:

```
Correct characters: 4
Correct positions: 3
```

> This means that the Wordle play "ABCDE" has four correct characters (A, B, C, and D) and three of them are in the correct position (A, B, and C).

Okay, so [not a great start.](/chat-gpt-cannot-wordle/chat-gpt-cannot-wordle-2)

