# PRD.md template

Use exactly this structure. Headings and IDs stay in English; body text is in
the PRD language. Keep empty sections with "—" while Draft, never delete them.

## ID scheme

| Prefix | Meaning | Example |
|---|---|---|
| `US-nn` | user story | `US-01` |
| `AC-nn.m` | acceptance criterion m of story nn | `AC-01.2` |
| `NG-nn` | non-goal | `NG-03` |
| `NFR-<QUALITY>-n` | quality target | `NFR-SPEED-1` |
| `E-<Name>` | data entity | `E-Request` |
| `D-nn` | decision in the log | `D-04` |
| `Q-nn` | open question, with priority P1/P2/P3 | `Q-02 [P1]` |
| `M-n` | success metric | `M-1` |

IDs never change once given. A removed item is struck through, not renumbered.

Labels: `[hypothesis]` = chosen by default or by the assistant, not confirmed by
the user. Everything without a label was decided by the user.

---

```markdown
# PRD: <product name>

Version: <n> · Status: Draft | Ready · Updated: <YYYY-MM-DD>
Stage: MVP | Prototype | POC

## 1. Problem
- What goes wrong: …
- Who feels it: …
- Cost today: …
- After: "<quote from the user's point of view>"

## 2. Users
### Primary persona: <name, age, role>
- Goal: …
- Today: …
- Blocked by: …
- Tech level: …
- Source: real conversations | [hypothesis]
### Secondary user: …

## 3. User stories
### US-01 <short title> · Must
As a …, I want …, so that ….
- AC-01.1 Given … When … Then …
- AC-01.2 (edge case) Given … When … Then …
Check: automatic test | human review (<who, on what sample>)

## 4. Non-goals (not in V1)
| ID | Not in V1 | Why not (yet) |
|---|---|---|
| NG-01 | … | … |

## 5. MVP scope
- Must: US-01, US-02, …
- Should: …
- Could: …
- Won't (V1): NG-01, NG-02, …

## 6. Success criteria
### Quality targets
| ID | Quality | Target |
|---|---|---|
| NFR-SPEED-1 | Speed | … |
### Success metric (pilot)
- M-1: <N of M …> within <time>, measured by <who/how>
- Pilot: <who, how many, how long>

## 7. Data model
| Entity | Key fields | Links |
|---|---|---|
| E-Customer | … | has many E-Request |
Decisions forced by the model: D-nn, …

## 8. Open questions
| ID | Priority | Question | Blocks | Recommended answer |
|---|---|---|---|---|
| Q-01 | P1 | … | US-02 | … |

## Glossary
| Term | Meaning (one term = one meaning) |
|---|---|

## Decision log
| ID | Question | Chosen | Rejected options | Why |
|---|---|---|---|---|

## Instructions for the AI coder
- Build only the Must stories, one story at a time; each AC is a test.
- Do not build anything listed in Non-goals (NG-*), even "to be helpful".
- Items marked [hypothesis] are defaults: keep them easy to change.
- If something is unclear or missing, stop and ask; do not guess.
- Access rules are enforced on the server, never only in the UI.
- <blocking rules: "Do not implement US-xx until Q-yy is answered.">
- <lines from the stranger test>

## Changelog
- v1 <date>: first version.
```
