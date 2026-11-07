# Weak-spot checklist

Run after every answer (only the checks the answer touches) and on the whole
draft before assembly. Each finding becomes one question with options that fix
it. Also use this list when the user asks to review an existing PRD.

| # | Check | Symptom | Fix with a question that offers |
|---|---|---|---|
| 1 | Slogan instead of problem | "X needs to go digital", "communication is hard" | concrete events with frequency |
| 2 | Everyone instead of a persona | "all customers", "businesses", "users" | one role, size, situation |
| 3 | Untestable words | fast, easy, intuitive, accurate, secure, scalable, modern, user-friendly | a number or a rule |
| 4 | Contradiction | "no account" + "order history"; "offline" + "real-time sync" | which side wins, and what that implies for the data model |
| 5 | Decision the AI made | integrations, prices, platforms, tools the user never chose | ask, or mark `[hypothesis]` |
| 6 | Unmeasurable success | "customers will love it", "increase engagement" | N of M, within a time frame |
| 7 | Empty non-goals | "None" | a proposed list with reasons |
| 8 | Prototype called MVP | no real user doing real work in the pilot | a real pilot, or rename to prototype |
| 9 | No failure behaviour | nothing about AI/network/payment errors | save, resume, never mark incomplete as complete |
| 10 | Security on the screen | "the page hides others' data" | server-side check + database row lock |
| 11 | Fake Must | V1 still solves the problem without it | move to Should/Could |
| 12 | Criterion without data | "Then it works", no Given/When/Then | concrete input and expected output |

## Stranger test (before Ready)

Ask yourself: "An AI coder gets only this file in a fresh session. What are the
five things it most likely builds wrong or adds on its own?" Typical answers:
adds a feature from the non-goals, picks a sign-in method, invents a price or
default value, skips an edge case, puts an access rule only in the UI. Each
answer becomes a question or a line in "Instructions for the AI coder".
