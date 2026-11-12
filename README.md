# Skills

Skills I use to build and check products with coding agents.

## Install

```bash
npx skills@latest add tarunfy/skills
```

To install only this skill:

```bash
npx skills@latest add tarunfy/skills --skill persona-feedback-loop
```

## Why use it?

You stop seeing your own interface after the hundredth time. You know where everything is, which words mean what, and which rough edges to step around. A new customer knows none of that.

`persona-feedback-loop` replaces that familiarity with a panel that sees the product once. Reviewer agents use it as different kinds of customer and file harsh, specific feedback. Then an orchestrator reads every entry, hands the genuine ones to implementer agents with a plan, checks their work itself, and keeps a status sheet until every entry has an outcome.

The part that matters is the second half. A list of complaints is easy to produce and easy to ignore. This skill does not stop until each one is fixed and verified, or handed to you as a decision.

## Reference

- **[persona-feedback-loop](skills/persona-feedback-loop/SKILL.md)**: a synthetic customer review of your product, closed out to the last entry. Use it for fresh eyes before a launch, a QA sweep from several points of view, or to turn a pile of feedback into verified fixes.

## How it works

1. **Cast a panel.** Five to seven reviewers: specialists in the craft, who find what is wrong, and everyday users from other fields, who find what is confusing.
2. **Pilot, then run.** One reviewer goes first to prove the setup. The rest follow, each filing one problem per entry through the product's own feedback channel.
3. **Triage every entry.** Genuine, partly genuine, yours to decide, to check, or not a defect.
4. **Package the work.** Implementers each own their files. No two edit the same file at once.
5. **Verify.** The orchestrator writes no product code. It runs every check on the combined result and looks at the product itself.
6. **Report.** What changed, what needs your decision, and what could not be verified.
7. **Your decisions, then a second round.** The open questions are asked one at a time and the answers are built the same way.

## What a run looks like

The first run was on a bookmark manager with a web app and a marketing site.

| | |
| --- | --- |
| Reviewers | 6: a product designer, a design engineer, an interaction engineer, a finance analyst, a PhD researcher, a marketing manager |
| Entries filed | 73, through a feedback form built the same day |
| Round 1 | 52 fixed and verified, 17 left as decisions for the owner |
| Round 2 | 22 questions asked one at a time; 21 became work, 1 was closed as it was |
| End state | Every one of the 73 entries has an outcome in the status sheet |

The most useful finds came from verifying, not from reviewing:

- a 6px sideways scroll that appeared only once every fix was combined,
- a sample-data mode everyone assumed was a sandbox and that was writing to the real database,
- a promise on the marketing page (export to HTML, CSV and JSON) that the app only half kept.

## Good to know

The reviewers are models playing roles, so what they file are leads to check, not user research. The skill says so in every report, and it hands questions of positioning, pricing and new features to you as decisions instead of acting on them.

## What your agent needs

- The ability to run several agents at once (reviewers and implementers) and to message them.
- A browser it can drive, signed in to a test account or a session you opened for it.
- A development environment to write to. The skill never touches production.

Nothing in it is specific to one agent.

## License

MIT
