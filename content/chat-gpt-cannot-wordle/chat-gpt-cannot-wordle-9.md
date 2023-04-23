---
title: "Chat Gpt Cannot Wordle 9"
date: 2023-04-21T12:11:34-06:00
description: "Adventures with Chat-GPT, Part 9"
summary: "I run Chat-GPT's fifth try in a Rust REPL"
---
On the `DETER-EERIE` example, Chat-GPT's code panicked:

```
>> println!("{}", wordscore("deter", "eerie"));
thread '<unnamed>' panicked at 'attempt to subtract with overflow', /rustc/9eb3afe9ebe9c7d2b84b71002d44f4a0edac95e0/library/core/src/ops/arith.rs:212:45
stack backtrace:
   0: _rust_begin_unwind
   1: core::panicking::panic_fmt
   2: core::panicking::panic
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: evcxr::runtime::Runtime::run_loop
   7: evcxr::runtime::runtime_hook
   8: evcxr::main
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

You know when you are interviewing someone and it is just going nowhere, even though you have a genial and likeable candidate? Like that.

The problem is that Chat-GPT really can't program, notwithstanding the number of times I've had good starting code produced by it. The evidence is now in: Chat-GPT cannot Wordle.
