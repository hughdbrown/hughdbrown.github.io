---
title: "Chat Gpt Cannot Wordle 3"
date: 2023-04-21T11:38:49-06:00
description: "Adventures with Chat-GPT, Part 3"
summary: "I refine the prompt and CHat-GPT tries again"
---
I gave it the prompt:
> Write code that generates the mask for correct letters in a guess. For example,  show how to generate ".X.xx" for the guess DETER if the correct answer is EERIE.

Chat-GPT cheerily answered:

> Sure, here's an example Rust code that generates the mask for correct letters in a guess:

```rust
fn generate_mask(guess: &str, answer: &str) -> String {
    guess.chars()
        .zip(answer.chars())
        .map(|(g, a)| if g == a { g } else { '.' })
        .collect()
}
```

> To use this function, you would pass in the guess as the first argument and the correct answer as the second argument. The function will return a String containing a mask for the correct letters in the guess, using a dot (".") to represent incorrect letters and the correct letter for correct letters.
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
Mask: .E..E
```

> This means that the correct letters in the guess "DETER" are E and E, which are in the 2nd and 4th positions, respectively, in the correct answer "EERIE".

[This was the first of several facepalms.](/chat-gpt-cannot-wordle/chat-gpt-cannot-wordle-4)
