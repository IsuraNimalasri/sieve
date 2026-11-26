# CHARTER.md

The decision-making protocol for this repo. Referenced by [AGENT.md](./AGENT.md).

## The principle

The human makes the call on any decision that has more than one reasonable
answer. The agent's job is to surface the real options and their trade-offs,
clearly enough that a non-expert can choose — not to pick the "best" one and
move on.

## When this protocol applies

Anything with a genuine trade-off:
- Which crate/library to use
- Data structure or algorithm choice
- Concurrency strategy (e.g. `Mutex` vs atomics vs an external crate)
- Error handling approach
- How strict vs how lenient a check should be
- Testing strategy for a given piece
- Anything that's hard to undo later without a rewrite

## When it doesn't apply

Mechanical or single-best-answer stuff: formatting, naming that follows an
already-established convention, fixing an obvious typo, a widely-agreed-on
practice with no real second option worth presenting.

## The format to use

When a decision point is hit, present it like this:

> There are **N common ways** to do [X]:
>
> 1. **[Approach A]** — if you go this way: [what you gain, what you give up]
> 2. **[Approach B]** — if you go this way: [what you gain, what you give up]
> 3. **[Approach C]** — if you go this way: [what you gain, what you give up]
>
> (Recommendation, only if asked for one, stated separately from the options.)
>
> Which do you want to go with?

Rules for this format:
- **2–4 options, not an exhaustive survey.** If there are more than 4 real
  contenders, group the weak ones or drop them — don't dump a research paper.
- **State the trade-off axis explicitly** (e.g. "simplicity vs. performance",
  "flexibility vs. how much you need to learn right now") — not just a list of
  pros/cons with no organizing idea.
- **No decision is made until the human answers.** Don't proceed past this
  point in the same turn.
- **Recommendations are opt-in.** Give one only if asked, and keep it visibly
  separate from the neutral option list so it doesn't read as the decision
  already being made for them.

## Example

> There are 3 common ways to rate-limit per account in a Rust gateway:
>
> 1. **Hand-rolled counter behind a `Mutex`** — simplest to understand line by
>    line, but you own the correctness of the locking and it's easy to get a
>    race wrong.
> 2. **Atomic counters with a time-bucketed key** — faster, no lock contention,
>    but the logic for bucket rollover is fiddlier to get right by hand.
> 3. **`governor` crate** — battle-tested, handles the token-bucket math for
>    you, but you're learning to read someone else's abstraction instead of
>    building the concurrency primitive yourself.
>
> Which fits what you want out of this step — learning the concurrency
> mechanics firsthand, or getting a working limiter faster so you can move on
> to pattern matching?
