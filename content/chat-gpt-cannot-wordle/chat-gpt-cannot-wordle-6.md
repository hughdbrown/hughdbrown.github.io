---
title: "Chat Gpt Cannot Wordle 6"
date: 2023-04-21T11:50:23-06:00
description: "Adventures with Chat-GPT, Part 6"
summary: "Trying to run Chat-GPT's third try"
---
So let's try this out in the Rust REPL, `evcxr`:

```rust
>> fn generate_mask(guess: &str, answer: &str) -> String {
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
>> println!("{}", generate_mask("deter", "eerie"));
.x.xx
```

The code still generates the wrong mask for `DETER` and `EERIE`, despite what Chat-GPT claims. But we expected that: Chat-GPT is *still* producing a one-pass algorithm.

And here is a simple example for which it is challenged:

```rust
>> println!("{}", generate_mask("chase", "erase"));
..XXx
```

You can see that the correct mask is `..XXX` but it marks the last `E` as in the wrong place, presumably because of the leading `E` in `ERASE`.

So [I tried again](/chat-gpt-cannot-wordle/chat-gpt-cannot-wordle-7)
