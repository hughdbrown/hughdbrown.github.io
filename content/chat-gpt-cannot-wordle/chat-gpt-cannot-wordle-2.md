---
title: "Chat Gpt Cannot Wordle 2"
date: 2023-04-21T11:32:32-06:00
description: "Adventures with Chat-GPT, Part 2"
summary: "What Chat-GPT got wrong the first time"
---
There are several things wrong with this. First, I wanted Chat-GPT to tell me which letters were:
- in the word and in the correct spot
- in the word but not in the correct spot
- not in the word at all.

Wordle players customarily do this with squares colored green, yellow, and white, respectively, to score, but since I was doing an API, I had a different idea. I wanted a string with:
- `X` to score a correct letter in the correct place
- `x` to score a correct letter in the wrong place
- `.` to score an incorrect letter.

Apart from this miscommunication on scoring, I could tell that this code could not possibly generate the right answer. I know from experience that you need two passes over the correct word to score a guess, and this one-pass could not be right.

So [I tried again.](/chat-gpt-cannot-wordle/chat-gpt-cannot-wordle-3)
