# Example: a finished PRD

Shows the level of detail to aim for: concrete numbers, every Must with
criteria, hypotheses labelled, open questions visible. Copy the depth, not the
domain.

```markdown
# PRD: Garage Intake Assistant

Version: 1 · Status: Ready · Updated: 2026-11-06
Stage: MVP

## 1. Problem
- What goes wrong: car owners book repairs by phone with vague symptoms
  ("strange noise"). Mechanics re-call 1–2 times per car to ask the model,
  mileage and when the noise happens; 1 in 5 cars arrives on the wrong day
  because the job needs a part that was not ordered.
- Who feels it: owners of independent garages with 2–6 mechanics, and their
  customers.
- Cost today: ~4 h/week of calls for the service manager; idle lifts when a
  part is missing.
- After: "Every booking arrives with the car, the symptoms and the photos. I
  order the part before the car comes in."

## 2. Users
### Primary persona: Dana, 41, service manager of a 4-mechanic garage
- Goal: confirm a booking with the right day and parts within one working day.
- Today: phone notes on paper, WhatsApp photos scattered across chats.
- Blocked by: missing car details and unclear symptoms.
- Tech level: WhatsApp and a shared Google Calendar; will not learn a new
  garage system.
- Source: talks with Dana and two other garage managers.
### Secondary user: car owners like Yossi, 58, who describe the problem.

## 3. User stories
### US-01 Describe the problem in one chat · Must
As a car owner, I want to describe my car and its problem in one chat, so that
the garage can book me without calling back.
- AC-01.1 Given Yossi wrote "Mazda 3, noise when braking" and gave no year
  When he taps "Send to garage" Then model: "Mazda 3" · year: "missing, ask at
  drop-off" · symptom: "noise when braking".
- AC-01.2 (edge case) Given Yossi does not know the mileage When the assistant
  asks Then he can answer "don't know" and the field is marked "missing", never
  guessed.
- AC-01.3 Every field links to the chat message it came from.
Check: automatic test for AC-01.1–01.3; Dana reviews 10 test chats for
question quality.

### US-02 Manager inbox with a WhatsApp alert · Must
As Dana, I want each finished booking request in one inbox, with a WhatsApp
alert, so that I see it without opening another app.
- AC-02.1 Given a request was sent When 2 minutes pass Then it is in the inbox
  and one WhatsApp message with a link reached Dana's number.
- AC-02.2 (edge case) Given the AI provider fails mid-chat When Yossi returns
  to the same link Then the chat continues where it stopped, and nothing is sent
  to Dana until he taps "Send".
Check: automatic test.

### US-03 Record the booking answer · Must
As Dana, I want to record the proposed day and estimate for a request, so that
the customer gets one clear answer.
- AC-03.1 Given request 41 When Dana enters "Tue 12 Nov, ₪600–900" Then the
  request status becomes "answered" and the customer sees it via the link.
Check: automatic test.

## 4. Non-goals (not in V1)
| ID | Not in V1 | Why not (yet) |
|---|---|---|
| NG-01 | Price from the AI | Depends on what the mechanic finds; a wrong price costs money and trust |
| NG-02 | Diagnosis from noise recordings | Unreliable; needs its own POC |
| NG-03 | Parts ordering integration | Different product: running the garage, not taking bookings |
| NG-04 | Calendar sync | Dana enters the day by hand in V1; [hypothesis] she accepts this |
| NG-05 | Payments | Paid at the garage |

## 5. MVP scope
- Must: US-01, US-02, US-03
- Should: upload photos of the dashboard warning light
- Could: export a request as PDF
- Won't (V1): NG-01 … NG-05

## 6. Success criteria
### Quality targets
| ID | Quality | Target |
|---|---|---|
| NFR-SPEED-1 | Speed | Assistant's next question starts within 3 s |
| NFR-SPEED-2 | Speed | Finished request in the inbox within 2 min |
| NFR-SEC-1 | Security | Customer signs in with a one-time SMS code; sees only own requests; checked on the server |
| NFR-PRIV-1 | Privacy | Phone, plate number, photos deleted 12 months after the job or on request; never used to train a model |
| NFR-COST-1 | Cost | < $0.10 model cost per finished request |
| NFR-FAIL-1 | Failure | Chat saved after every message; incomplete request never shown as complete |
| NFR-AVAIL-1 | Availability | Evenings and weekends matter most; 1 h downtime acceptable in the pilot |
### Success metric (pilot)
- M-1: Dana books 8 of 10 requests without calling the customer, over 4 weeks,
  counted by Dana in the inbox ("called: yes/no" flag).
- Pilot: every new booking inquiry at Dana's garage for 4 weeks, ~40 customers.

## 7. Data model
| Entity | Key fields | Links |
|---|---|---|
| E-Customer | name, phone | has many E-Request |
| E-Request | date, status (draft/sent/answered) | has many E-Message, many E-Car, one E-Answer |
| E-Message | author (customer/assistant), text | — |
| E-Car | model, year, mileage, plate number, symptoms (each with source E-Message) | — |
| E-Answer | day, estimate range, written by Dana | — |
Decisions forced by the model: D-02, D-03.

## 8. Open questions
| ID | Priority | Question | Blocks | Recommended answer |
|---|---|---|---|---|
| Q-01 | P3 | Wording of the "Send to garage" button | — | "Send to garage" |

## Glossary
| Term | Meaning |
|---|---|
| request | one customer's booking inquiry, may cover several cars |
| answer | the day and estimate Dana sends back; not a final price |

## Decision log
| ID | Question | Chosen | Rejected options | Why |
|---|---|---|---|---|
| D-01 | Who talks to the assistant? | Customer only | Mechanics entering phone bookings | Phone bookings can wait for V2 |
| D-02 | Two cars, one request or two? | One request, several cars | One per car | Dana answers a family in one message |
| D-03 | Identify customer without account? | Phone + SMS code | Email; no identification | History and privacy need an identity |
| D-04 | Does the AI give a price? | No | Price from a table | NG-01 |

## Instructions for the AI coder
- Build only US-01, US-02, US-03, one at a time; each AC is a test.
- Do not build anything in NG-01 … NG-05, even "to be helpful".
- NG-04 is a [hypothesis]: keep the day as a plain field, easy to sync later.
- If something is unclear or missing, stop and ask; do not guess.
- Access rules are enforced on the server, never only in the UI.
- Never show a price produced by the model anywhere (NG-01).
- Never fill a missing field with a guess; mark it "missing" (AC-01.2).

## Changelog
- v1 2026-11-06: first version.
```
