---
name: prd
description: >-
  Turns a raw product idea into a complete PRD.md that an AI coding agent can
  build from without guessing. Runs a strict one-question-at-a-time interview:
  every question comes with a recommended answer and alternatives, vague answers
  are pushed back, contradictions and weak spots are flagged at once, and
  nothing the user did not decide is silently filled in. Saves progress to
  PRD.md so the interview can stop and resume. `/prd pack` also writes
  data-model.md, architecture.md, adr/001-*.md and critique.md. Use whenever
  the user types /prd, asks to write, finish, review or continue a PRD, a
  product spec, requirements or an MVP scope for an idea, or says things like
  "напиши PRD", "ТЗ для ИИ", "спецификация для идеи", "grill me on my idea",
  even if they do not say "PRD".
argument-hint: "[pack] [idea in a few words]"
---

# PRD interview

Goal: one file, `PRD.md`, in the current working directory, good enough that a
stranger, or an AI coder in a fresh session, can start building the MVP from it
alone. Code is cheap; knowing what to build is the expensive part. A model asked
to "write a PRD" fills every gap with the average answer for that kind of app.
This skill reverses the roles: you ask, the user decides, the file records it.

Arguments: `$ARGUMENTS`
- starts with `pack` → pack mode (section 8) after the PRD is ready;
- any other text → the idea; use it as the answer to start question 3 and
  skip that question.

Reference files in this skill's folder, `${CLAUDE_SKILL_DIR}`; read each one when
the step says so, not up front:
- `references/phases.md`: what to ask in each phase, with weak-answer examples.
- `references/checklist.md`: the 12 weak-spot checks.
- `references/prd-template.md`: the exact PRD.md layout and ID scheme.
- `references/pack-templates.md`: layouts for the four pack files.
- `references/example-prd.md`: a finished PRD (car repair shop) showing the
  level of detail to aim for. Read it once before writing the first draft.

## 1. Language and tone

- Chat language: the user chooses it in the very first question (section 2).
  Everything the user reads in the chat is in that language: every
  question, `header`, option label and description, "Why it matters" line,
  status, checkpoint and closing message. This skill's files are in English;
  translate their wording, never copy it verbatim into a question. The PRD
  language choice affects only the files, never the chat.
- PRD language: asked in start question 2. Every file is written in it.
  IDs (`US-01`), section headings in the template and code identifiers stay in
  English whatever the language, so an AI coder can grep them.
- Plain short sentences. One question, one decision.

## 2. Start

Check the working directory for `PRD.md` first.

**Always first: the chat language.** Ask, in English only (this is the one
message written before the choice exists): "[1/40] In which language should I
ask the questions?" Options, labels exactly as written, each in its own
script: "English", "Русский", "עברית". Do not add an "Other" option: the UI
adds it automatically, and the user types any other language there. The chosen
or typed language becomes the chat language. Recommend nothing. Say nothing else before
this question. From the answer on, use only the chosen language in the chat.
This question counts as question 1. Do not confirm the choice or wait: in the
same response, ask question 2 (the next one below).

**No PRD.md** — then ask, one at a time (numbers continue the count):
2. PRD language. Options, always these three, labels in their own script:
   "English" (recommended: AI coders work best with it; add the
   "(Recommended)" suffix in the chat language), "Русский", "עברית".
   Descriptions are in the chat language. Do not add an "Other" option: the UI
   adds it automatically, and the user types any other language there.
3. (skip if `$ARGUMENTS` gave the idea) "Describe the idea in 2–3 sentences:
   what, for whom, why now." Free text: ask in plain text, not with options.
4. Restate the idea in one sentence: who has which problem, and what V1 does
   about it. Ask: "Is this right?" Options: yes / adjust.

Then go straight to phase 1 (Problem), first P1 question.

**PRD.md exists** — read it. Use its PRD language for the files; do not ask
again. Then ask question 2:
- `Status: Draft` → continue: the first open phase, then open questions
  P1 → P2 → P3 (recommended);
- check hypotheses: confirm or replace every `[hypothesis]`;
- deepen a section (the user picks which);
- start a new PRD: rename the old file to `PRD.v<N>.md` first, never delete it.
- `Status: Ready` and pack mode → go to section 8 without this question.

## 3. How to ask (every question)

Use the `AskUserQuestion` tool for every question that has options. Exactly one
question per call, one call per message. Never bundle two decisions into one
question: the point is that each answer is a real decision the user made.

- `header`: the phase name, max 12 characters (`Problem`, `Persona`, `NFR`…,
  in the chat language).
- `question`: starts with `[<n>/<limit>]`, then the question, then one short
  line "Why it matters: …". It must stand on its own: the user may see only the
  dialog.
- `options`: 2–4. The first is your recommendation, its label ends with
  "(Recommended)" in the chat language. Each `description` names the trade-off.
  Make options concrete: numbers, names, real choices, not "yes/no/maybe".
- `preview`: the exact line(s) this option would write into PRD.md, e.g.
  `NFR-SPEED-1: first reply within 3 s`. This shows the consequence before the
  user commits.
- "Other" is added automatically; the user can type free text or a control word
  there.

Free-text questions (the idea, a quote, a list of entities) are asked in plain
text, still one per message.

**Control words** (in any language, typed via "Other" or as a message):
`back` (change the previous answer), `skip` (send the question to Open
questions), `status`, `draft` (show the current PRD), `stop` (save and end).
Mention them once, in the message after the start questions.

**After each answer**, before the next question, write one line:
`Recorded → US-02` or `Conflict with US-01: <why>`. Then ask the next question.

**Never stop between questions.** "One call per message" means one question
per response, not one question per session. Every response after an answer
ends with the next question, in the same response: no "shall I continue?",
no waiting for "continue". A response ends without a question only after the
closing message (section 7) or on `stop`. Reading a reference file or saving
PRD.md is part of the same response; ask the next question after it.

## 4. Rules that make the interview work

- **Push back on vague answers.** "Fast", "easy", "everyone", "modern",
  "secure", "scalable" cannot fail, so they cannot pass. Ask a follow-up whose
  options are numbers or concrete rules.
- **Never decide silently.** If the user says "you decide" or "don't know", take
  your recommended option, mark it `[hypothesis]` in the PRD, and move on. The
  user can confirm hypotheses later. Anything you add that the user did not
  choose is a hypothesis too.
- **Run the weak-spot checks after every answer**, not only at the end
  (`references/checklist.md`). A problem found now is one question; found after
  coding it is a rewrite.
- **Contradictions reopen sections.** If a new answer breaks a closed section,
  name the section and the clash, and ask whether to reopen it.
- **Scope creep guard.** A new feature in the middle of the interview gets one
  question: Must or Won't? If Must, what moves out?
- **Test every Must**: "Drop it. Does V1 still solve the problem?" If yes, it
  was not a Must.
- **MVP means real users doing real work.** If no real user will use V1 for
  real work, say it is a prototype, not an MVP, and record that.
- **Product before tools.** Do not ask about frameworks, databases or hosting
  in the PRD. If the user insists on a stack, record it as a constraint, not a
  decision you made.

## 5. Phases

Read `references/phases.md` now. Order:

0. Idea (start questions)
1. Problem: four answers (what, who, cost today, what "after" looks like)
2. Persona: one primary user, one secondary; hypothesis label if not from real
   conversations
3. Stories + acceptance criteria (Given / When / Then) + edge cases
4. Non-goals + MoSCoW cut
5. Quality targets (NFR): security, behaviour on failure, speed, privacy,
   cost, availability
6. Data model: entities from the nouns in the stories, links, hidden questions
7. Success metric: a business number with a deadline
8. Assembly and self-critique

Inside each phase, ask in priority order: P1 first, then P2, then P3.
- **P1, blocking**: without it an AI coder cannot build a Must, or builds it
  wrong (who may see what, how users sign in, what "done" means).
- **P2, important**: affects quality but has a sensible default.
- **P3, minor**: can be decided during development.

Before each question, give it a priority. Ask a P2 only when the phase has no
open P1, and a P3 only when it has no open P1 or P2. Implementation details
(where a list is stored, who edits a config file, exact field formats) are P3:
do not ask them before the P1s; send them to Open questions.

**Save after every closed phase**: write `PRD.md` with `Status: Draft`, using
`references/prd-template.md`. Also save at each checkpoint and on `stop`.
The file is the memory; the chat may be compacted.

## 6. Length guard

Count questions in this session (start questions included).

- **Checkpoint every 15 questions.** Save PRD.md, show a 3-line status: closed
  sections, open P1/P2/P3 counts, hypotheses. Ask one question: continue
  (recommended while P1s are open) / go to closing mode / save and stop.
- **Soft limit: 40 questions** (15 in pack mode). Tell the user the limit is
  reached and enter closing mode.

**Closing mode:**
1. Ask each open **P1**, one at a time, with your recommended answer first so
   it can be accepted in one click.
2. If the user answers "don't know" to a P1, try once more with a narrower
   question: two extreme options, or "which of these is definitely wrong?".
   If still open, keep it open; do not invent the answer.
3. All open **P2**: one single question: "Accept my recommendations for these N
   items as `[hypothesis]`?" List the items in the question text and in
   `preview` (one line each). Options: accept all / go through them one by one
   / leave them open.
4. Open **P3**: move to Open questions without asking.
5. Any P1 still open → `Status: Draft`, and in "Instructions for the AI coder"
   add: "Do not implement US-xx until Q-yy is answered."

The counter restarts when the user runs `/prd` again; the limit is per session.

## 7. Assembly and Ready gate

Before the final write:
1. Run all checks in `references/checklist.md` against the whole draft.
2. Stranger test: list five things an AI coder would most likely get wrong
   with this file.
   Give each finding from steps 1–2 a priority. Each P1 becomes its own
   question. All P2 and P3 together become one question, as in closing mode
   step 3. Every question counts toward the limit. An accepted finding becomes
   a PRD line or a line in the coder instructions.
3. Set `Status: Ready` only if all hold:
   - every Must story has at least one Given/When/Then criterion with concrete
     data;
   - every NFR has a number or a rule;
   - Non-goals is not empty, and every item has a reason;
   - the success metric is measurable and has a deadline;
   - no open P1 question.
   Otherwise `Status: Draft`, and tell the user what is missing.
4. If `PRD.md` already has `Status: Ready` and content would change, ask
   before overwriting: overwrite, or save as a new version (bump `Version`, add
   a line to the Changelog).

End with: the path, the status, counts (stories, criteria, open questions,
hypotheses), and one line: "Start coding in a fresh session and give it only
PRD.md." A long interview chat is cluttered; the file is the clean copy.

## 8. Pack mode (`/prd pack`)

Writes four more files next to `PRD.md`: `data-model.md`, `architecture.md`,
`adr/001-<slug>.md`, `critique.md`. Read `references/pack-templates.md`.

1. **No PRD.md** → run the full PRD interview first, then continue here.
2. **PRD.md has open P1 questions** → offer to close them first (recommended):
   architecture built on a PRD with holes repeats the holes.
3. Separate counter, limit 15 questions, same checkpoint and closing rules.
4. Files, in this order, each saved as soon as it is done:
   - `data-model.md`: entities, fields, links with cardinality, a Mermaid ER
     diagram that follows the diagram rules in the template. Before saving,
     check that the diagram and the tables have the same entities and links.
     Ask only what the PRD left open.
   - `architecture.md`: six layers (browser, frontend, API, service, data
     access, database) plus external services; trace the main user request
     down and back up; an API contract per main endpoint (address, who may
     call, inputs, output, error codes 401/403/422); where authorization is
     checked (server, plus row-level security in the database as a safety
     net; never "hidden on screen"); which jobs run in the background and why.
     Default to one app with modules (a monolith). Any component beyond that
     must cite the PRD line that needs it.
   - `adr/001-<slug>.md`: pick the most disputed decision from the PRD's
     decision log, propose it to the user, fill the five fields. Ask "Revisit
     when" as its own question: it needs a concrete fact or number.
   - `critique.md`: ask the user to paste an architecture another AI proposed.
     If they have none, write a deliberately over-built one yourself
     ("ready for millions of users": microservices, autoscaling, message bus,
     a database per service, features from the non-goals), label it
     illustrative, then reject it item by item: proposed → why not → which PRD
     or ADR line rules it out → what instead.
5. Stack choices (which database, which framework) are "what to build it with",
   not "what to build". If the user wants one fixed, record it as ADR-002 with
   the same five fields.
