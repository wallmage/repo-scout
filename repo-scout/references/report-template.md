# Scout Report Template

Use this structure. Keep the whole report to one to two pages for a single target and at most three pages for a very large repository. The step-by-step mechanism is the main body; cut audit bookkeeping and background before cutting the explanation of how the target works.

Write for a reader with no technical background — someone's mom or dad, busy, deciding whether this thing is worth their time. Every sentence must make sense to that reader on first pass. Write in the user's language and translate everything, including the verdict words; only the 🟢/🔴 icons, web links, and file paths stay as they are.

Plain-words rules for the whole report:

- No unexplained technical terms. When a real name is unavoidable (a program they must have, an account they must create), say what it is in everyday words in the same sentence.
- Describe benefits and drawbacks from the experience standpoint, never the technology standpoint: not "a self-hosted web service with persistent state" but "it runs on your own computer, remembers your past work, and your data never leaves your machine."
- Do not use internal artifact or architecture labels as explanations. Terms such as "evidence ledger," "contradiction map," "citation audit," "question tree," "orchestration," and "multi-agent" tell a newcomer nothing by themselves; say what the user sees the tool do instead: "it keeps a list showing which source supports each claim" or "it gives the same question to several independent researchers, then compares their answers." If a real name must appear, explain it immediately in the same sentence.
- No audit bookkeeping ever appears in the report: no commit hashes, resolution chains, snapshot or worktree state, symlink notes, file or line counts, and no lists of what was or wasn't read. All of that goes to the deep-dive notes.
- No numeric scores and no rubric axis names — the rubric is reasoning, never output.

The report contains no security commentary. The single exception is clear evidence of deliberate malice, which becomes the verdict's decisive reason on line one.

---

# Scout Report: <name>

**Verdict: 🟢 INSTALL | 🔴 SKIP** — <on green: the distinctive mechanism and its concrete effect; on red: the decisive failure>

The verdict block is three to five short sentences in total, plain enough to read aloud.

For 🟢, use this exact information order:

1. First sentence: name the distinctive mechanism and the result it produces.
2. Second sentence: explain why that mechanism produces a better result than the obvious alternative.
3. Third sentence: name the concrete situation where the user would reach for it.
4. Final sentence: say whether it runs on their machine. When only part is worth having, name the installed part, what the rest of the repository contains, and why those other parts are not needed.

Passing an audit gate is not a user benefit and never belongs in a green opening. Mention a failed gate only when it causes a red verdict.

Verdict-line examples: `🟢 INSTALL — it splits one research question among several investigators, cross-checks their evidence, and combines it into one sourced report.` · `🔴 SKIP — the author has abandoned this (no meaningful updates in 8 months, and it depends on a service that no longer exists); use <alternative> instead.` · `🔴 SKIP — there is no code behind this link to look at, so nobody can tell you whether it is worth installing.`

That last case — trigger 8, Unverifiable — is the one red that is about missing evidence, not about a fault. Say plainly what was looked for and not found, never imply the software is bad, and never let the project's own marketing stand in for the source nobody could read.

## What it is

Two to four sentences: what the user gives it, what comes back, and when the user would choose it over doing the job normally. Describe the target itself, not its ancestry or implementation trivia. Do not tell the reader that it imitates, replaces, or omits another project unless that fact changes what they can do, what they must install, or what they must pay.

## How it works — step by step

Write 10–15 numbered steps. Trace one real use from the user's request to the finished result. Every step contains two things in plain language:

1. **What happens:** who or what does an observable action.
2. **Why it helps:** why that step improves the result, catches a mistake, saves work, or makes the outcome easier to trust.

Use the form: `<plain action> — <why that step improves the result>`. Do not use a stage name as its own explanation. For a simple target, explain each step more concretely. For a complex target, combine low-level actions into larger stages while preserving the full input-to-result chain. Base the steps on the files that implement the mechanism, not the README's feature list.

## Why you'd want it

Two to four short bullets. Each bullet states, in order: the ordinary problem, what the target changes, and the practical consequence for the user. A feature count is not a benefit. "It has two checks" says nothing; say what mistake those checks prevent and what happens without them. Compare against the obvious alternative with a concrete difference in the result, time, effort, cost, or failure risk.

## Watch out for

At most two bullets, and only things that would genuinely change the user's week: real money it costs, accounts they must create, heavy upkeep, a rough or confusing experience, a core feature that doesn't match the marketing. Cosmetic issues, minor documentation mistakes, and normal signs of an actively developed project are never worth the reader's time. When nothing meets that bar, omit the section.

## On your machine

Answer "will this run for me, and how?" with one recommended way — never a menu.

- Whether it runs on this computer: what is already in place, what is missing, and the one-line command or step to get each missing piece with the user's package manager. If probing was impossible (no shell), say so plainly and keep the recommendation generic.
- Name the exact installation scope and explain why that scope contains everything needed for the recommended use. When installing only one folder from a larger repository, explain what the excluded repository areas are for and why they do not affect the installed target. When the whole repository is required, explain which parts depend on one another. Never state "install only this folder" without the reason.
- Recommend exactly one way to run it, chosen by the experience rule and the effort rule ([install-playbook.md](install-playbook.md)): the full point-and-click experience whenever one exists, reached in the fewest user steps. When nothing graphical exists, say plainly: "there is no app window — you use this by typing commands."
- If running it takes several parts working at once, that is plumbing the user should never see: the plan is one start command or start-on-login, ending with one address to open or one icon to click — never terminal windows to keep open.
- Other documented setups get at most one sentence — "there are other ways to install it meant for developers; you don't need them" — with no list.
- What you must have first: accounts or keys in plain words, and what they cost when the project says.
- End with what success looks like: "when it's ready, a page opens in your browser at <address> and you'll see <what>."

Derive everything from the project's own install docs and files; describe, never execute during the audit. If the project's own instructions are unclear or wrong, say so in plain words — that is honest, report-worthy evidence.

Close the report with one line, `Source: <link>`, then follow the existing approval state (SKILL §9 and [install-playbook.md](install-playbook.md)). If the user's request was an audit only, add the single assisted-install offer, naming the one recommended way. If the user gave an imperative install, setup, or adoption request, do not ask again; continue with the approved installation after the report. No offer after an unapproved 🔴.

## The deep dive — researched, never volunteered

The sections above are the entire visible report. The audit still produces the developer-level material; keep it ready and print none of it. When the user says **"deep dive"** — or "advanced info", "deeper info", or otherwise clearly asks for developer-level detail — reply with it in full:

- The mechanism traced properly: one concrete run from trigger to output — state, handoffs, verification, completion.
- The active ingredients and the filler: which components survive the removal test, which are decoration.
- A 10-minute reading map: 2–4 real file paths in reading order, each with what to notice.
- Worth stealing: 2–4 reusable design patterns.
- Compatibility and ownership detail: platform constraints, dependencies, install footprint, maintenance evidence, context cost.
- Audit bookkeeping: exact revision, the resolution chain, worktree or content-hash state, and what was left unread or sampled.

Never advertise that this level exists; the reader who needs it knows the words.
