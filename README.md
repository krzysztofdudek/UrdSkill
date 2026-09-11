<img width="1376" height="768" alt="Urd — the well at the root of the tree: your AI coding agent consults the source of truth and asks instead of guessing when the spec runs out" src="https://github.com/user-attachments/assets/47fb992b-c367-45e9-87af-58610b26e525" />

# Urd

**Your AI agent hit a gap in the spec, so it guessed, and shipped.** Urd makes it stop and ask instead.

The moment the agent moves from a plan into code and the plan runs out, Urd makes it **consult the source of truth or ask you** — instead of inventing an answer and burying the decision in the diff.

```
/plugin marketplace add krzysztofdudek/UrdSkill
/plugin install urd@urd-marketplace
```

Run both, then `/reload-plugins` to activate it in this session (or restart Claude Code). No config, no API key. The skill activates on its own the moment the agent moves from a plan, design doc, or spec into implementation.

> MIT licensed · single markdown file · works with any agent that reads skills · part of the [Yggdrasil family](#the-yggdrasil-family) · [full skill body](skills/urd/SKILL.md)

---

## See it

**Step 4 of the plan: implement the retry logic.** The plan says, verbatim:

> *"On 5xx response, retry up to 3 times with exponential backoff."*

Mid-implementation the agent hits a **429 (rate-limited)** — not a 5xx, but retryable. The spec is silent on it. The default agent picks something and moves on. Urd makes it stop:

> **Question — retry logic, step 4.** The plan covers 5xx, but I've hit **429 (rate-limited)**: not 5xx, yet retryable. The spec doesn't say how to treat it. Three options:
>
> 1. Treat 429 like 5xx — retry with backoff.
> 2. Honor the `Retry-After` header if present, otherwise back off.
> 3. Leave 429 to the caller — don't retry it here.
>
> I'd go with (2). Proceed?

**Note what it did:** it didn't resolve the ambiguity silently. Ten seconds on a decision now, instead of finding it in the diff an hour later. That's the whole skill — at the moment of doubt, ask, don't guess.

---

## When it fires

The skill activates the moment the agent moves from a plan/spec into implementation. Once active, it pushes toward asking rather than guessing whenever:

- the spec is **silent** on a case the agent hit
- the spec **contradicts itself**
- the agent is about to add a **fallback, exception, or `TODO`** the spec didn't sanction
- a **test would have to be weakened or skipped** to make the code pass
- a **framework or type-system constraint** is forcing a workaround
- the agent catches itself thinking *"this is probably fine"* without certainty

Well-written plans trigger few stops. Sparse plans trigger many. That's the point.

---

## How it asks

When Urd asks, it gives you enough to decide in seconds — never an open-ended *"what should I do?"*:

| It tells you | Example |
|---|---|
| **Where it is** in the plan | "Step 4, retry logic" |
| **What the spec says**, verbatim | "On 5xx, retry 3× with backoff" |
| **What it found** that doesn't fit | "hit a 429, not covered" |
| **The options**, with trade-offs | the numbered list above |
| **Its recommendation** | "I'd go with (2)" |

It also **distinguishes verified from believed**: it states as fact only what it has checked against the source. An unverified inference — a cause, a mechanism, how an API behaves — it labels as a guess or verifies before you rely on it. Challenge a claim and it re-reads the source and corrects itself, instead of defending an answer it never confirmed.

---

## Install

### Claude Code plugin (recommended)

Two slash commands. The first registers this repo as a marketplace; the second installs the plugin from it.

```
/plugin marketplace add krzysztofdudek/UrdSkill
/plugin install urd@urd-marketplace
```

Then run `/reload-plugins` to activate it in the current session (or restart Claude Code). No config, no API key.

To upgrade later, refresh the marketplace and reinstall:

```
/plugin marketplace update urd-marketplace
/plugin install urd@urd-marketplace
```

### GitHub Copilot CLI plugin

The same repo is also a [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli) marketplace. Register it, then install the plugin:

```
copilot plugin marketplace add krzysztofdudek/UrdSkill
copilot plugin install urd@urd-marketplace
```

To upgrade later: `copilot plugin update urd`. The same skill body powers both Claude Code and Copilot — nothing changes in how it behaves.

### Codex CLI plugin

Codex reads the same skill. Register this repo as a marketplace, then install:

```
codex plugin marketplace add krzysztofdudek/UrdSkill
codex plugin install urd@urd-marketplace
```

To upgrade later: `codex plugin marketplace upgrade urd-marketplace`. Or drop the single file into `~/.agents/skills/urd/SKILL.md` (user-level) or `.agents/skills/urd/SKILL.md` (project-level).

### Cursor plugin

Cursor auto-discovers the skill from the plugin manifest at the repo root. Install it locally:

```
git clone https://github.com/krzysztofdudek/UrdSkill.git
ln -s "$(pwd)/UrdSkill" ~/.cursor/plugins/local/urd
```

Then reload Cursor (**Developer: Reload Window**). Or drop the single file into `~/.cursor/skills/urd/SKILL.md` (user-level) or `.cursor/skills/urd/SKILL.md` (project-level).

### Single-file drop-in (any agent)

The whole skill is one frontmatter-tagged markdown file: [`skills/urd/SKILL.md`](skills/urd/SKILL.md). Copy it into your agent's skill directory.

- **Claude Code, user-level:** `~/.claude/skills/urd/SKILL.md`
- **Claude Code, project-level:** `.claude/skills/urd/SKILL.md` in your repo
- **Other agents:** wherever your tool reads markdown skills

Nothing else in this repo affects behavior — all of it lives in that one file.

---

## What it doesn't claim

Urd is a skill that biases the agent toward asking — not a runtime guarantee it will always stop. It does not eliminate bugs, and it does not fix a vague spec for you: it surfaces the gaps so *you* can. If what you want is a fast pass with deviations resolved silently, this is deliberately the wrong tool — it trades clarification rounds for fewer wrong outcomes delivered confidently.

---

## FAQ

<details>
<summary><b>Won't this make the agent constantly stop?</b></summary>

Only when the spec doesn't cover the case. The skill carries an explicit ask-vs-proceed table — if the spec answers the question, it proceeds; if it doesn't, it asks. Most well-written plans don't trigger many stops. Sparse plans do — and that's the signal you wanted.
</details>

<details>
<summary><b>Does it conflict with TDD, debugging, or verification skills?</b></summary>

No — it composes. It governs the *attitude* moving from spec to code, not the process. TDD still says write the test first; Urd says ask when the test you'd write isn't covered by the spec.
</details>

<details>
<summary><b>What if I'd rather the agent just ship something?</b></summary>

Then don't install it. It's deliberately the opposite of "ship something." A correct outcome after a few clarification rounds beats a wrong outcome delivered without questions — but if you want the fast pass, this is the wrong tool.
</details>

<details>
<summary><b>Why "Urd"?</b></summary>

Urð is the Norn who keeps the well at the root of Yggdrasil — the source of truth consulted before acting. The skill plays the same role for your codebase: the plan is the source of truth, not the agent's judgment. It's part of the [Yggdrasil family](#the-yggdrasil-family).
</details>

---

## The Yggdrasil family

**Three jobs, one core, in layers.** **[Yggdrasil](https://github.com/krzysztofdudek/Yggdrasil)** is the law: the architecture graph and the rails that hold every change to it. **[Grain](https://github.com/krzysztofdudek/Grain)** surveys the terrain: it mines that graph from a repository's own code and history, so there is a rule-backed map before anyone writes a rule by hand. **[Horde](https://github.com/krzysztofdudek/Horde)** is the software house that builds on the law: zero standing roles, a worker per ticket and a one-shot architect who rules the whole plan once, each ticket refined onto the graph and given a tick. Adoption runs Grain first — install it day zero for a soft, draft-only law that never blocks — then Yggdrasil as the core you keep long term, hard law with proof and CI; Horde is the one door for any mission too big for one agent, not a second path that only opens once a lone agent runs out of room. From 6.0.0 the core ships as one version; the add-ons keep their own. In the family, law is raised by whichever agent does the work in its own territory, and only the client — the one person the whole system answers to — lowers or vetoes it. The three repositories' shared machine contracts are registered on [one page](https://krzysztofdudek.github.io/Yggdrasil/family-contracts).

| Core | What it holds |
|---|---|
| **[Yggdrasil](https://github.com/krzysztofdudek/Yggdrasil)** | The law. The architecture graph and the rails that hold every change to it, checked before the agent moves on, re-proved in CI without a key. |
| **[Grain](https://github.com/krzysztofdudek/Grain)** | The terrain survey. Mines a repository's own code and history into a first graph — components, dependencies, and the rules the code already keeps, each with the count of places that break it today; Yggdrasil accepts it with one command. |
| **[Horde](https://github.com/krzysztofdudek/Horde)** | The software house on the law. Zero standing roles: a worker per ticket in its own worktree, refined onto the graph and given a tick by a nine-item merge checklist; a one-shot architect rules the whole plan once; the client orders the mission and is the only one who can lower or veto a rule. |

Three add-ons attach to the agent rather than to the graph, and each works alone. Horde doesn't assume any of them is installed — it carries its own minimum discipline in each role's law — but uses them when they are, one sentence per row below.

| Add-on | Stage | What it makes the agent prove | In Horde's loop |
|---|---|---|---|
| **[Ratatoskr](https://github.com/krzysztofdudek/RatatoskrSkill)** | request → intent | Keeps the agent talking to you in plain words, not code, so you can follow what it's doing. | Keeps the client's plain-language registry open at both ends of a mission. |
| **Urd** (this one) | intent → code | When the spec is ambiguous, it consults the source of truth and asks, it doesn't guess. | The stop a worker hits before it guesses. |
| **[Researcher](https://github.com/krzysztofdudek/ResearcherSkill)** | code → measured result | Point it at a metric and it runs experiments, hypotheses kept and discarded. | Runs the retrospective's measurement. |

## License

MIT © [Krzysztof Dudek](https://github.com/krzysztofdudek)

---

<div align="center">
  <img src="yggdrasil.svg" alt="Yggdrasil" width="150" />
</div>
