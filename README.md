<p align="center">
  <img src="assets/logo.png" width="220" alt="A corgi, head tilted, holding a rubber duck in its mouth, entirely unbothered">
</p>

<h1 align="center">gu-ask-real</h1>

<p align="center">
  <em>You asked for the ball.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2FHin-Nattapat%2Fgu-ask-real%2Fmain%2F.claude-plugin%2Fplugin.json&query=%24.version&prefix=v&label=plugin&color=6f42c1&style=flat&labelColor=555" alt="plugin version">
  <img src="https://img.shields.io/badge/license-MIT-cfc493?style=flat&labelColor=555" alt="MIT">
  <img src="https://img.shields.io/badge/always--on-207%20tokens-2b2b2b?style=flat&labelColor=555" alt="207 tokens always-on">
  <img src="https://img.shields.io/badge/apologies-0-e0a33e?style=flat&labelColor=555" alt="zero apologies">
</p>

---

Coding agents rarely fail by writing a bad line. They fail by drifting: the third attempt at
the same function, the fix that quietly undoes the last fix, the confident "done" for something
that was never run. By the time you notice, you are not annoyed at one edit — you have lost
track of what is even true in the working tree.

Then you say it. *Wait — what are you doing?* — the sound a person makes when what came back
is not what they asked for.

And the agent apologises for two paragraphs and rewrites everything, which is worse.

`gu-ask-real` is what should happen instead.

## Before / after

**Without.** You point out the problem. The agent opens with an apology, agrees enthusiastically,
and starts a large rewrite to demonstrate that it listened. The rewrite touches files nobody
asked about. You now have two problems.

**With.** No apology. It stops editing, and hands you this:

```
original ask : "add the branch filter to the report endpoint"
where we are :
  uncommitted     : 4 files, +310/-96
  committed here  : a1b2c3d wip: filter, 9f8e7d6 fix filter, 4c5b6a7 revert filter
  last good point : 9f8e7d6 — filter worked, before the caching change
why it circled : assumed the report shared the order repo's cache. it does not.
A  keep going from here : rip the cache out of the report path · ~30 min
B  go back to 9f8e7d6    : lose the caching work · ~5 min · filter still works
C  keep the filter, drop the cache commits · ~10 min
```

Then it waits. **You** pick — you are not the one who lost the thread.

## When it fires

You can always invoke it. It also fires on itself, but only on things that can be **counted**,
never on a feeling:

- the **3rd attempt** at the same file, behaviour or question
- it is about to **undo something it wrote itself** earlier in the session
- you have **restated the same request twice**

"It feels like this is going badly" is not a criterion. A count is.

## Three shapes, because one form does not fit

| | | |
|---|---|---|
| **T1** | one miss | Four lines: asked / did / gap / fix. Then it fixes it. A T1 that needs more than four lines was a T2. |
| **T2** | circling | **Touches nothing.** Facts from `git status`, `git diff --stat`, `git log`. Names the last good point. Offers keep-going / go-back / keep-some with costs, and stops. |
| **T3** | the real problem is bigger | Says so out loud, with what the real fix costs and what it touches — then stops. **Never quietly starts the bigger fix** because it happens to be correct. The size of the work is your call. |

That last one matters more than it looks. "Smallest possible fix" is the right instinct for a
typo and the wrong one for a design that is already wrong; the skill has to be able to say
*this needs to be bigger* without being allowed to just go and do it.

## Never

- **Never apologise.** Not a paragraph, not a line. It changes nothing and takes the space the facts needed.
- **Never edit while diagnosing.** Editing while confused is what produced the confusion.
- **Never blame the prompt.** It may genuinely have been ambiguous. Still not the answer.
- **Never rewrite everything to show it listened.** The most common reaction, and the worst.
- **Never fire twice in a row.** It stopped, you answered — now do the thing you picked.

## The ledger

Every firing appends one line to `~/.claude/gu-ask-real.md`:

```
2026-09-03 · T2 · proposed something already implemented · read the file before proposing
```

It is **global on purpose**. What goes wrong follows the person, not the repository, and a
ledger that resets with every checkout can never see a second occurrence.

A prevention is proposed as a real rule **only when a matching line is already there** — the
second time, quoting both dates. A rule built from one incident is usually the wrong rule, and
a file full of them is a file nobody reads.

## Install

### Claude Code — as a plugin

```bash
claude plugin marketplace add Hin-Nattapat/gu-ask-real
claude plugin install gu-ask-real@gu-ask-real
```

Or from inside a session, as two separate prompts:

```
/plugin marketplace add Hin-Nattapat/gu-ask-real
```
```
/plugin install gu-ask-real@gu-ask-real
```

### Claude Code — as a plain skill

```bash
# every project
npx skills add Hin-Nattapat/gu-ask-real -g -a claude-code --skill '*' -y

# this project only
npx skills add Hin-Nattapat/gu-ask-real -a claude-code --skill '*' -y
```

Pick one or the other — installing both leaves two copies in the skill list.

### Other agents

It is a plain [Agent Skill](https://agentskills.io) — swap `-a claude-code` for `-a codex`,
`-a cursor`, `-a opencode`, or any other host the
[`skills`](https://github.com/vercel-labs/skills) CLI supports. The T2 path assumes a git repo.

### Update

```bash
claude plugin update gu-ask-real     # plugin install (restart to apply)
npx skills update -g                 # plain-skill install
```

`claude plugin update` compares the `version` in `plugin.json`, not the commit — a push without
a version bump installs nothing.

### Uninstall

```bash
claude plugin uninstall gu-ask-real@gu-ask-real
claude plugin marketplace remove gu-ask-real
# or, for a plain-skill install:
npx skills remove gu-ask-real
```

## The name

Thai. *กุถามจริง* — roughly "am I seriously asking this right now", said at the screen, out loud,
by someone who has read the same wrong answer three times.

## License

MIT
