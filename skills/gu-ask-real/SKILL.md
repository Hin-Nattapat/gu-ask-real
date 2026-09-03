---
name: gu-ask-real
description: Use the moment the work has gone off the rails — the answer missed what was asked, something was claimed that was never run, or the same thing has been attempted round after round. Shows what was asked, what actually happened, and where the last good point is, then stops for the human. Never apologises, never rewrites everything to look responsive. Triggers on "wait, what", "why did you do that", "that's not what I asked", "you're going in circles", "stop", "start over", "กุถามจริง", "ทำไมทำแบบนี้", "งงแล้ว", "วนอยู่ได้".
---

# gu-ask-real

*The sound a person makes when what came back is not what they asked for.*

This skill does not fix things. It says what happened, shows where you can stand, and stops.

## When it fires

The user says so — or **one of these is objectively true**:

- this is the **3rd attempt** at the same file, behaviour or question
- you are about to **undo something you yourself wrote** earlier in this session
- the user has **restated the same request twice**

Nothing countable is true and the user hasn't said anything → **do not fire**. "It feels like this is going badly" is not a criterion; a count is.

## Classify first — the form depends on it

| | |
|---|---|
| **T1 · one miss** | Misread the ask · edited the wrong thing · claimed it worked without running it. Nothing below applies. |
| **T2 · circling** | Any firing criterion above hit, or the uncommitted diff has grown every round with nobody checking it. |
| **T3 · the real problem is bigger** | The correct fix has to leave what was asked — another layer, another repo, or the structure itself. |

## T1 — four lines, no more

```
asked   : <quote the user, not your paraphrase>
did     : <what actually happened + file:line or line count>
gap     : <one line. no apology, no "I should have">
fix     : <the smallest change that closes it>
```

Then do it. A T1 that takes more than four lines to describe was a T2.

## T2 — do not touch a single file

Editing while confused is what produced the confusion. Gather facts, not narrative:

```bash
git status --porcelain          # what is dirty right now
git diff --stat                 # how much, in which files
git log --oneline -8            # what got committed during this
```

```
original ask : <the FIRST message that asked for this, quoted — not the latest one.
                the original is what got buried; the latest is already downstream of the mess>
where we are :
  uncommitted     : <files + lines, from --stat>
  committed here  : <sha + subject, this session only>
  last good point : <sha, or "before this thread started">
why it circled : <one line — which assumption was wrong, and from which round>
A  keep going from here : what still has to change · cost
B  go back to <point>   : what gets thrown away · cost
C  keep <x>, drop <y>   : cost
```

**STOP.** The user picks. Not you — you are the one who lost the thread.

## T3 — say the size out loud

```
in flight  : <the patch being attempted>
why it is not enough : <one line — what it leaves broken or hidden>
the real fix : <what it actually takes> · cost <rough> · touches <files / repos / layers>
```

Then **STOP**. Never quietly start the bigger fix because it is the right one — the size of the work is the user's call, always.

## Never

- **Never apologise.** Not a paragraph, not a line. It changes nothing and costs the space the facts needed.
- **Never edit while diagnosing.** T2 especially: hands off the keyboard.
- **Never blame the prompt.** It may well have been ambiguous. That is still not the answer.
- **Never rewrite everything to show you listened.** This is the most common reaction and the worst one.
- **Never fire on yourself twice in a row.** Fired once, stopped, and the user answered? You are done — do what they picked.

## The rule — only when it has happened twice

Every firing appends **one line** to `~/.claude/gu-ask-real.md` (create it if missing). The ledger is deliberately **global, not per-project** — what goes wrong follows the person, not the repository, and a ledger that resets with every new checkout can never see a second occurrence.

```
YYYY-MM-DD · T<n> · <what went wrong, ≤12 words> · <what would have prevented it, ≤12 words>
```

Read the file before writing. **A matching line is already there → this is a repeat**: propose the prevention as a real rule for the project's agent instructions, quoting both dates. First occurrence → log the line and propose nothing.

A rule built from a single incident is usually the wrong rule, and a file full of them is a file nobody reads.
