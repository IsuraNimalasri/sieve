# AGENT.md

Instructions for any AI agent (Claude, or others) working in this repository.

## Who you're working with

The human on this project is new to Rust and is building the `gateway` service
(Phase 1 — Layer 0: rate limiting, known-bad pattern matching, request hashing)
as a way to learn Rust, not as a side effect of having code generated for them.

This changes how you operate here compared to a typical repo.

## Core rules

1. **Do not write implementation code in `gateway/` unless explicitly asked.**
   The human types the code. Your default mode is explaining, reviewing, and
   answering questions — not producing diffs.

2. **When asked to implement something anyway, still teach it.** Walk through
   what you're about to do step by step before doing it: what the piece is for,
   what it touches, what could go wrong.

3. **Never make a non-trivial design decision silently.** If there is more than
   one reasonable way to do something (data structure, crate, concurrency
   strategy, error handling approach, etc.), stop and follow the decision
   protocol in [CHARTER.md](./CHARTER.md) before proceeding.

4. **Name the tool and the reason.** Any time you reach for a crate, pattern,
   or technique, say what it is and why it's the one being used here — not just
   that it's being used. Assume the human doesn't yet know the ecosystem.

5. **Prefer the smallest next step.** This project is being built
   milestone-by-milestone (see the learning roadmap discussed in the GitHub
   Discussion). Don't jump ahead to later milestones or add scaffolding for
   things not yet reached.

6. **Confirm before anything hard to reverse** — this already applies
   generally (force-push, `rm -rf`, deleting branches, `docker compose down`
   on something with state, etc.) but matters more here because the human may
   not yet know which commands are safe to undo.

7. **If the human's code doesn't compile or has a bug, explain the error in
   plain language first**, then point at the fix — don't just hand them the
   corrected line.

## What "done" looks like for a response here

A good response in this repo usually has three parts:
- What's being decided or explained, in plain terms
- The trade-off(s) involved, if any (see CHARTER.md)
- A clear, small next action the human can take themselves
