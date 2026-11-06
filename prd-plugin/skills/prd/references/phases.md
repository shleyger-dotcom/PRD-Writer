# Phases: what to ask

For each phase: the goal, the questions in priority order, what a weak answer
looks like, and when the phase is closed. Adapt the wording to the idea; do not
read the list aloud. Ask in the user's chat language: these English examples
are for you, translate them. Skip a question when an earlier answer already settled it.

## 1. Problem

Goal: a specific pain, felt by specific people, with a cost today.

| Pri | Question | Weak answer → push back with |
|---|---|---|
| P1 | What exactly goes wrong today? Describe the last time it happened. | "Communication is hard", "needs to go digital" → options naming concrete events: "requests arrive with missing details", "orders get lost", … |
| P1 | Who feels it? | "Everyone", "businesses" → narrow: role, size, situation ("owners of 1–5 person shops") |
| P2 | What does it cost today: time, money, lost customers? | "A lot" → ranges: <1 h/week, 1–5 h, >5 h; or "N of 10 orders go wrong" |
| P2 | What will the user say when it works? | Feature list → a one-sentence quote: "Every new request arrives complete. I just write the price." |

Closed when: all four answers are concrete; the "after" quote exists.

## 2. Persona

Goal: one typical user built from real people, plus the secondary user.

| Pri | Question |
|---|---|
| P1 | Who uses V1 directly? Only one role in V1 is the usual right answer. |
| P1 | Their goal, in one line with a number ("send a quote within a day"). |
| P2 | What they do today, and what blocks them. |
| P2 | Tech level: what they already use daily (WhatsApp, Excel…); what they refuse to learn. |
| P2 | Is this from real conversations? If not → label the persona `[hypothesis]`. |
| P3 | Secondary user (e.g. the customer on the other side). |

Weak: a marketing segment ("people 25–45"). Push for one named, typical person.

## 3. Stories and acceptance criteria

Goal: each story says who wants what and why; each criterion is a check that
passes or fails.

Story: `As a <persona>, I want <action>, so that <outcome>.`
Criteria: `Given <situation with concrete data> / When <action> / Then <result>`.

Order: first one question per story until every V1 story is drafted (both
sides, e.g. the customer sends a request and the owner receives it). Then, per
story, the happy-path and first edge-case criteria. Only then the P2 items.

Ask per story:
- P1: the story itself (offer 2–3 drafted versions as options).
- P1: the happy-path criterion, with concrete data.
- P1: the edge case the persona will hit first: missing data, unknown
  measurement, duplicate submission, user leaves mid-way, AI unavailable.
- P2: where data comes from and how it is traced (e.g. "every field links to
  the message it came from").
- P2: what is marked missing instead of guessed.

Also ask once per PRD: which checks are automatic (code tests run every time)
and which need human judgment (e.g. "the owner reviews 10 chats").

Weak: "Then it works correctly", "Then the user is happy" → ask what exactly
appears on the screen, in the database, or in a message.

Closed when: every Must story has ≥1 happy-path and ≥1 edge-case criterion.

## 4. Non-goals and MoSCoW

Goal: write down what V1 will not do, with reasons; sort every feature.

- P1: propose a non-goal list yourself: the features an AI would add "to be
  helpful" for this kind of app (pricing, payments, integrations, 3D, admin
  panels, analytics, mobile apps, other product types). One question per
  disputed item, or one question "accept these as non-goals?" with the list in
  the preview.
- P1: Must list. Test each: "Drop it. Does V1 still solve the problem?"
- P2: Should / Could.
- Every non-goal gets a reason: "why not (yet)".

Weak: "None" for non-goals → there are always some; propose them.

## 5. Quality targets (NFR)

One number or rule per quality. Offer concrete options.

| Pri | Quality | Example target |
|---|---|---|
| P1 | Security / access | Sign-in method; who can see whose data |
| P1 | Behaviour on failure | AI or network down: save progress, resume later, never mark incomplete data as complete |
| P2 | Speed | First AI reply within 3 s; finished item reaches the owner within 2 min |
| P2 | Privacy | What personal data, how long it is kept, deletion on request, never used for training |
| P2 | Cost | Max model cost per finished item, e.g. < $0.10 |
| P3 | Availability | When it matters most; acceptable downtime in the pilot |

Weak: "fast", "secure", "reliable" → the number or rule.

## 6. Data model

Goal: a map of what the app remembers. The nouns in the stories become the
entities; arrows mean "has many".

- P1: confirm the entity list (offer it in the preview).
- P1: the hidden questions the sketch forces. Typical ones:
  - one request with several items, or one request per item?
  - how is a user identified without an account?
  - can a record be edited after it is sent? Who sees the old version?
  - what happens to records when a user asks for deletion?
- P2: key fields per entity.
- P3: statuses and their transitions.

Decide on paper, before any database exists.

## 7. Success metric

Goal: a business number for the pilot.

- P1: the pilot: who, how many users, how long (e.g. "every new inquiry for
  four weeks, about 30 customers").
- P1: the one behaviour to watch, as a number: "the owner quotes 8 of 10
  requests without calling back". What people do matters more than what they
  say.
- P2: how it will be measured (who counts, from what).

Missing a criterion is a defect; missing the business number is a lesson.
Record both kinds separately.

## 8. Assembly

See SKILL.md section 7: checklist pass, stranger test, Ready gate.
