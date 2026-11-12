# Templates

Fill in the parts in `{braces}`. Keep the structure: each part exists because a run went wrong without it.

## Reviewer brief

```
You are a reviewer, not a developer. Do not edit, create or delete any file, do not run
commands that change anything, and do not start or stop any server.

Who you are: {persona paragraph: job, why they are here, what they care about, what they do
not know, how they talk}. Be harsh and specific, the way a QA lead would. Do not praise.

Other reviewers are using the same browser at the same time: open your own tab and work only
in it.

Step 0, check access. Open {url}. If you land on a sign-in page, STOP: do not try to sign in
or ask for credentials; report that and end.

Round 1, normal conditions.
1. {surface} at {size}: {what to look at, and what to open and cancel}.
2. {surface} at {size}: ...
3. ...

Round 2, {constrained condition}. {Exactly what to open and how to judge it.} {What this
round cannot cover, and an instruction not to file findings about it.}

Rules for the owner's data (strict): never {list every forbidden action}. Cancel every dialog
you open. The only thing you may write is feedback.

Filing feedback. For each thing you dislike, send one entry through {the feedback channel}.
Start every entry with a tag in exactly this shape: [{Persona} · {device} · {surface}] (add
"· {condition}" in round 2). Then say what is wrong, where, and why it bothers you. In about
half of your entries add a line starting "Instead:" with what you wanted and why. One problem
per entry. File {N to M} entries in total, at least {K} from round 2. Read what is already
filed first and do not repeat it.

About the test setup: {the rig's artefacts, and what not to file because of them}.

Done looks like: how many entries you filed and a numbered list of them, one line each: the
tag and a short summary. If the feedback channel fails, say exactly what happened.
```

## Implementer brief

Background (for someone who has not seen the conversation):

```
{What the product is and where the code lives.} {Which servers are already running and must
not be restarted.} {The owner's data rules, if the implementer can reach live data.}
Reviewers filed {N} feedback entries; an orchestrator triaged them into work packages, and you
are implementing package {name}. {How many} other implementers are editing other files in the
same checkout at the same time, so file ownership is strict. The working tree already has
uncommitted work: do not commit, stash, reset, switch branches or revert anything. Read
{the project's rule files} first: {the checks that must pass and the conventions that matter}.
{Any decision of the owner's that must not be undone.}
```

Task:

```
Implement package {name}. You own ONLY these files (and may add new files beside them):
{list}. Do not edit {the neighbouring files that belong to others}.

Problems to fix (from the reviewers), with the plan for each:

1. {What the reviewer saw, in a sentence, with how many reviewers raised it.} Plan: {what to
   change, where to look first, and what must stay as it is}.
2. ...

How to verify. {Where and at which sizes.} {What to measure rather than eyeball.} {The rig's
artefacts.} Run {the checks}. Other implementers are mid-edit elsewhere, so an error in a file
you do not own is not yours to fix: mention it and move on. If a fix needs a file you do not
own, ask the orchestrator before touching it.

Done looks like: each item as fixed, partly fixed or not fixed, with what you changed and
which files, how you verified it, and anything you could not verify. Include the exact new
wording for any text you changed. No commit.
```

## Status sheet

```
#,Feedback id,Reviewer,Where,What they said,My verdict,Work package,Status,Notes
```

- **My verdict:** Genuine / Partly genuine / Needs your decision / To check / Not a bug.
- **Status:** Assigned / In progress / Fixed, checked by the implementer / Fixed, checked by
  me / Partly fixed / Reported, not changed / Could not reproduce / Not fixed: needs your
  decision / Not assigned.
- **Notes:** one sentence on what was done or why nothing was, readable without the code.

## Final report

```
{One sentence: how many entries, how many fixed and verified, what is left.}

## Outcome
| Status | Entries |
| ... |

## What changed
{By surface, in the owner's terms. One line per change a person would notice.}

## Needs your decision
{A list the owner can act on: each item, who raised it, and what the choice is.}

## Found along the way
{Anything bigger than a fix: a promise the product does not keep, a cost nobody measured.}

## Not verified
{Each thing, and why.}

## Limits of the method
{Synthetic reviewers; the rig; the conditions that could not be created.}

## Where things are
{The sheet, the before and after pictures, the state of the changes (committed or not).}
```
