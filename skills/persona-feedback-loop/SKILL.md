---
name: persona-feedback-loop
description: Run a full synthetic customer review of a product and close the loop on it. Reviewer agents use the product as different kinds of customer (design experts and everyday users from other fields), file harsh, specific feedback through the product's own feedback channel, and then an orchestrator triages every entry, hands the genuine ones to implementer agents with a plan, verifies their work, and keeps a status sheet until every entry has an outcome. Use when the user wants fresh eyes on an app or site, a QA sweep from several points of view, "be harsh on my product", persona testing, a mock user panel, a pre-launch review, or wants reviewer feedback turned into verified fixes. Triggers on persona review, synthetic users, user panel, fresh users, harsh feedback, QA sweep, dogfood, "what would customers hate", "review it like a designer", feedback form, triage feedback, feedback loop, before and after.
---

# Persona feedback loop

A product team reads its own interface so often that it stops seeing it. This skill replaces that familiarity with a panel of reviewers who each see the product once, from a different seat, and then makes sure what they found is either fixed and verified or handed to the owner as a decision. It has two halves that can be run together or apart:

1. **Review.** Reviewer agents use the product as customers and file feedback.
2. **Close the loop.** An orchestrator triages every entry, assigns the genuine ones to implementers, verifies the results and reports.

The orchestrator in the second half **writes no product code**. Its work is reading, judging, planning, checking and reporting. That separation is what keeps the verification honest.

## What this is and is not

- It is a way to surface defects, confusions and missing pieces quickly, from more points of view than one person holds.
- It is **not user research.** The reviewers are models playing roles. Treat what they file as leads to check, never as evidence of what real customers want. Say this in every report.
- Entries about positioning, pricing, naming and new features are real signals, but they are the owner's decisions. The loop surfaces them; it does not act on them.

## Before starting: settle five things

Ask the user only for what you cannot find out yourself.

1. **What is under review.** Which surfaces (app, marketing site, docs, onboarding) and which URLs or builds.
2. **How reviewers get in.** A session the user has already signed in to, a test account, or seed data. Never ask for a password, never copy a login token from one place to another, and never sign in on the user's behalf. If a constrained test would need the sign-in moved somewhere else, that part of the test is out of scope: say so.
3. **What reviewers may touch.** Default: read-only, apart from filing feedback. Spell out the forbidden actions for this product (saving, deleting, sending, publishing, changing settings, signing out, joining a list).
4. **Where feedback goes.** The product's own feedback form if it has one. If it has none, either build a minimal one first (see below) or use a shared file as the intake. Prefer the product's own channel: it tests that channel too.
5. **Which environment gets written to.** Feedback rows, uploads and any schema change go to a development environment. Confirm which one before the first write, and never touch production.

**Do not assume a sandbox; prove it.** A demo mode, a sample-data switch or a "test" label says nothing about where a save goes. Before telling any agent it may write freely somewhere, read the code path a save takes or watch the request it sends. Until a sandbox is proven, every surface is read-only.

## Phase 0: the intake

If the product has no feedback channel and the user wants one, build the smallest thing that works, following the project's own conventions:

- an entry point that is always reachable (and reachable on a phone),
- a short form: a description, an optional picture,
- storage with the same access rules as the rest of the product,
- the page it was sent from, saved with the entry,
- a plain list of what has been sent, even if temporary.

Send one test entry yourself and confirm it is stored and displayed before any reviewer runs. Tell the user it is there.

If you use a file instead, one entry is one block: tag, what is wrong, where, why it matters, and an optional "Instead:" line.

## Phase 1: cast the panel

Five to seven reviewers is enough. More mostly produces duplicates. Mix two groups:

- **Experts in the craft**, who will find what is wrong: for an interface, a product designer (hierarchy, copy, empty states, consistency), a visual or motion-minded design engineer (spacing, states, loading, polish), an interaction engineer (keyboard, focus, touch, accessibility).
- **Everyday users from other fields**, who will find what is confusing: people who would plausibly use the product but share none of its makers' vocabulary. Give each a job, a reason to be there, habits, and the tools they already use. Their value is that jargon stops them.

Write each persona as a short paragraph: who they are, what they care about, what they do not know, and how they talk. Tell every reviewer to be harsh and specific, and not to praise.

## Phase 2: pilot, then run

**Run one reviewer first.** The pilot exists to answer practical questions before you spend the whole panel: can a separate agent reach the signed-in product, does the feedback channel accept its entries, are the rules clear. If the pilot is not signed in, it must stop and report rather than try to sign in.

Then run the rest together. Each reviewer gets a brief with:

- its persona,
- the steps (which surfaces, in which order, at which sizes),
- the rules for the owner's data, stated as a strict list,
- how to file: one problem per entry, a fixed tag at the start, what is wrong, where, why it bothers them, and in about half the entries an "Instead:" line with what they wanted and why,
- how many entries to file (a range, such as 8 to 14),
- an instruction to read what is already filed and not repeat it,
- what the finished report looks like.

**Tags.** If reviewers share one account, the tag is the only way to tell entries apart. Use one shape for all of them, for example `[Persona · device · surface]` and `[Persona · device · surface · slow network]`.

**Rounds.** Run each reviewer under normal conditions first, then under constrained ones (a slow network, a small screen, reduced motion, a keyboard only). Be exact about what "constrained" means in your setup and what it cannot cover. Do not let a reviewer file findings about a condition it could not actually create.

**Know your test rig's artefacts**, and name them in the brief so they are not filed as defects: developer toolbars, an embedded browser that does not draw animation frames, emulated sizes that do not hold, a first load that is slow only because it is a development build.

## Phase 3: triage every entry

Read every entry in full, not a summary. Give each one a verdict:

| Verdict | Meaning | What happens |
| --- | --- | --- |
| Genuine | A defect, a confusion or a rough edge that can be fixed without a product decision | Assigned |
| Partly genuine | Part is a fix, part is a feature or a preference | The fixable part is assigned; the rest goes to the owner |
| Needs the owner's decision | Positioning, pricing, naming, a new feature, or something the owner chose on purpose | Not assigned; listed for the owner |
| To check | A claim that may be wrong or may be an artefact | Assigned as an investigation: confirm before changing anything |
| Not a defect | An artefact of the rig, or a misreading | Recorded with the reason |

Things to watch for:

- **Agreement is the strongest signal.** When three reviewers with different backgrounds hit the same thing, it is real.
- **Check anything the owner did deliberately** against the conversation history before calling it a defect.
- **Reviewers are sometimes wrong.** A claim about what the page does (a control is focusable, a number is missing) needs confirming.
- **Copy that states a fact must be true.** If a fix rewrites what the product says it does, the implementer must verify the statement against the code.
- **"The product lacks X" is a claim too.** Before reporting a missing feature to the owner, search the code and the menus for it. A reviewer who did not find an export does not prove there is none.

## Phase 4: the status sheet

Keep one sheet, one row per entry, from triage to the end. A CSV in the repository is enough. Columns:

`#, feedback id, reviewer, where, what they said, verdict, work package, status, notes`

Statuses move in one direction: Assigned, then Fixed and checked by the implementer, then Fixed and checked by the orchestrator; or Partly fixed, Reported without a change, Could not reproduce, Needs the owner's decision. When a fix cannot be exercised without changing the owner's data, give it its own status ("Fixed, not verified with a real save") instead of rounding it up to checked. Update the sheet every time a result comes back, not at the end. The notes column says what was actually done, in a sentence the owner can read without the code.

## Phase 5: package the work

Group the assigned entries into work packages. The rule that matters most:

**No two implementers working at the same time may edit the same file.** Divide by files and areas, not by theme. Then:

- List in each brief the files that implementer owns, and the neighbouring files it must not touch.
- If two packages need the same file, run them one after the other.
- Expect an implementer to find that a fix belongs in a file it does not own. It should ask the orchestrator, who grants it if nobody else owns that file and otherwise queues a follow-up.
- Collect the follow-ups implementers report ("this also needs changing in a file I do not own") into a short last wave.
- A message to a busy implementer waits until its current turn ends. A correction that cannot wait (a safety rule, a wrong assumption in the brief) goes to every affected implementer at once, and you check afterwards what each did before it arrived.
- When two packages want the same key, name or setting, the orchestrator decides and tells both. Implementers should raise the clash, not settle it between themselves.

Each implementer brief contains:

- the background a newcomer needs: what the product is, where the code lives, the project's rules, what is already in the working tree,
- the rules for the owner's data,
- the owned files,
- for each item: what the reviewer saw, the plan for fixing it, and anything that must stay as it is,
- how to verify it, including the rig's artefacts,
- what the finished report looks like: each item as fixed, partly fixed or not fixed, with what changed, how it was checked and what could not be checked,
- "do not commit".

Give implementers the strongest model available for this step. The plan and the judgement of what is genuine come from the orchestrator; the implementation quality comes from them.

## Phase 6: verify

A report from an implementer is a claim. Before a row moves to "checked by the orchestrator":

1. **Run every check on the combined result**, not package by package: lint, types, tests, any structural checks, and a production build if there is one. Several implementers working at once make the machine slow and tests time out; re-run failures alone, and then the full suite on a quiet machine, before deciding they are real.
2. **Look at the product yourself** for the fixes that matter most, at the sizes the reviewers used. Measure what can be measured (is the text visible on the first frame, where does focus go, does the page scroll sideways) instead of trusting a screenshot.
3. **Hunt for regressions that combination causes.** Each package may be correct alone and wrong together. Re-measure the basics after every wave: sideways overflow at the narrowest size, console errors, the first screen.
4. **Send regressions back to the implementer who has the context**, with the measurement and where it was taken.

Capture a before and an after picture for the changes a person can see. Take the "before" pictures before the first implementer starts.

**Schema changes get extra care.** One implementer owns each change to the database. It is applied to the development environment only, checked straight away by reading through the product's own queries (a new link between two tables can break every existing join), and written to be re-runnable. Record what was applied and under which version, because a tool may record it differently from the file's name.

**A refused action stays refused.** If an agent's action is declined (by a permission prompt or a tool) and it asks another agent or the orchestrator to do it instead, nobody does it. The orchestrator stops the agent, tells the owner what was asked and why, and acts only on the owner's own answer.

## Phase 7: report

The report is for someone who was not watching. Lead with the outcome, then:

- how many entries, and how many ended in each status,
- what was fixed, in the owner's terms, grouped by surface,
- **what needs the owner's decision**, as a list they can act on,
- anything the review uncovered that is bigger than a fix (a promise the product does not keep, a cost nobody had measured),
- what could not be verified and why,
- the limits of the method: synthetic reviewers, the rig, the conditions that could not be created,
- where the sheet, the pictures and the uncommitted or committed changes are.

## Phase 8: the owner's decisions, then a second round

The entries left for the owner are usually the most valuable ones, and a list of seventeen open questions rarely gets answered. So ask them:

- **One at a time**, each with what the reviewers said, what it would cost, and a recommendation. Record the answer in the sheet as it is given.
- "Leave it as it is" is an answer. Close the row with the reason.
- Then run phases 5 to 7 again on the answers. These are features, not fixes, so the briefs need more: the wording the owner approved, what must not change, and the tests that stand in for anything that cannot be exercised on the owner's data.
- Copy changes that depend on a fact the owner has not confirmed (what a plan includes, what ships at launch) are worded to be true either way, and the open fact goes back to the owner as a question.

## Rules that hold throughout

- **The orchestrator writes no product code.** If something needs changing, it goes to an implementer.
- **Reviewers and implementers never change the owner's data**, beyond filing feedback. Opening a dialog and cancelling it is looking; pressing its confirm button is not.
- **Never move credentials.** A session that is signed in can be reused in place; it cannot be copied.
- **Development environment only** for anything written.
- **A sandbox is proven, never assumed.**
- **A declined action is never routed through another agent.** It goes to the owner.
- **Nothing is committed** until the owner has seen the result, unless they said otherwise.
- **Say what was not done.** A screen that was not opened, a condition that could not be created, a claim that could not be reproduced: each one is named.

## What a run looks like

From the run this skill was written after (a bookmark manager with a web app and a marketing site):

- 6 reviewers: three design specialists and three everyday users (finance, research, marketing).
- 73 entries filed through a feedback form built the same day.
- Triage: 52 genuine or partly genuine, 19 for the owner to decide, 1 artefact of the rig, 2 to check.
- 8 work packages across two waves, with strict file ownership and no commits.
- Outcome: 52 entries fixed and verified, with one promise the product did not keep discovered along the way (an export feature the marketing page described and the app did not have).
- One regression caught only by re-measuring the combined result: a 6px sideways scroll at the narrowest phone width.
- Round 2: the owner answered 22 open questions one at a time; 21 became work for 6 more implementers (sorting and filters, collection covers and safer sharing, a shortcuts sheet, several links at once, a Notion import, an export page, and a rewrite of the marketing copy and pricing card), and 1 was closed as it was.
- Final sheet: 38 entries checked by the orchestrator, 27 by their implementer, 4 fixed but not exercised with a real save, 3 waiting on one answer from the owner, 1 closed.

Templates for the reviewer brief, the implementer brief and the report are in [TEMPLATES.md](TEMPLATES.md).
