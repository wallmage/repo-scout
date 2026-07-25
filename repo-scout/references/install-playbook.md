# Install Playbook

Everything here happens **after** a verdict is formed. None of it runs during inspection, and none of it is needed on an audit-only run.

Read this before probing the user's machine and again before taking any installation action. The boundaries in SKILL.md §7 and §9 bind whether or not this file was read; what follows is the procedure for honouring them.

The plain-words rules from the report template apply to everything the user sees here, including step-by-step narration.

---

## Detect the environment (read-only)

Run this only when auditing on the user's real machine, after the verdict is formed. Nothing here installs, mutates, or starts anything.

Probe read-only, and probe only what matters. After identifying the target's documented install paths, check the host for exactly the prerequisites those paths name: OS and version, CPU architecture, the user's package manager (Homebrew, apt, winget…), and the presence and version of each required runtime. Use `command -v x` / `x --version`-style checks only — never install, never mutate, never start a daemon, service, or network call while probing.

Then pick the one recommended path with two rules.

The **experience rule**: when the target ships any graphical way to use it — a web page it serves, or a desktop or mobile app — recommend the path that delivers that full point-and-click experience; a terminal-only variant is recommended only when nothing graphical exists, and the report must then say plainly that there is no app window and the tool is used by typing commands.

The **effort rule**: among paths that deliver the full experience, pick the one with the fewest steps and decisions for this user on this machine — "simplest" counts the user's steps, never the number of components installed, and it never justifies a cut-down version of the product; prefer runtimes the user already has when paths are otherwise equal.

That path becomes the **On your machine** recommendation in the report — name what is already satisfied, what is missing, and the one-line command to get each missing piece with the detected package manager.

If no shell is available, say probing was not possible and present the documented paths un-tailored.

---

## Offer assisted installation

This extends the pre-install gate; it never runs during inspection. If the user's request was an audit only, then after a 🟢 verdict, end the report with one question: whether the user wants the skill to install and set everything up, naming the chosen scope and the recommended **On your machine** path it would use. If the user gave an imperative install, setup, or adoption request, that original instruction is the per-session approval: continue after the 🟢 report and do not ask again. Approval for this session is either the user's original explicit install request or the single yes to that post-audit offer. That approval covers exactly the chosen scope, nothing else. Ask once; make no offer after an unapproved 🔴. An overridden 🔴 proceeds only after the user explicitly approves the override.

### Install only the exact snapshot that produced the verdict

- **Every Git source:** install from the content-addressed snapshot made from the exact source bytes before inspection, whether the source was clean or dirty. Verify its complete path-and-content hash manifest immediately before the first target command. The recorded commit SHA is source identity, not the installation payload; never substitute HEAD or a recorded commit SHA for the frozen bytes.
- **Archive, package, or remote script:** execute the saved bytes whose content hash was audited. When the installer must download an artifact, verify that artifact against the audited hash before opening or running it.
- **Version drift:** never re-fetch and execute a moving branch, tag, package label, or URL as a shortcut. If the exact audited bytes are unavailable or any SHA or hash differs, stop installation and audit the changed content before proceeding.

### Modes

On the approval defined above, carry it out by whichever mode the step and the harness allow:

- **Mode 1 — run the documented commands.** When the path is command-shaped, run its setup steps against the exact audited snapshot under the host's normal permission system; replace any moving clone or download step with the pinned local snapshot or hash-verified artifact defined above. Narrate each step. Install any missing prerequisite with the user's package manager as part of the run, naming each one before installing it. Narration follows the same plain-words rules as the report: each update names a concrete thing in everyday words ("now installing the part that shows the app in your browser — step 2 of 3"), never vague verbs like "patching things up".
- **Mode 2 — computer use for manual steps.** When steps are inherently manual (GUI installers, drag-to-Applications, browser downloads, settings dialogs) and the harness exposes desktop or browser control, drive those steps for the user, narrating. This is a capability fallback for manual steps, not a replacement for Mode 1 where commands exist.
- **Mode 3 — printed manual.** When there is no execution ability, or no computer use where a step needs it, produce a tailored, ordered, copy-pasteable manual for the detected environment — each step with its command or exact manual action, jargon glossed, and a final "how to know it worked" check. This is the universal fallback and is always possible.

### Stay on the recommended path

Install what the report recommended: the assisted install uses exactly the path named in **On your machine**. If something discovered mid-install forces a change, say in one plain sentence what the report got wrong, then continue on the corrected path — never switch silently, and never downgrade the user to a lesser experience (a terminal-only variant of a graphical product) for convenience. When the documented way to run the product needs several long-running parts, use the project's own single-command or service option, or set it to start on login as a named part of this approved install — never leave the user with terminal windows to keep open, and always end with the one address to open or icon to click.

### Hard boundaries, all modes

The approval defined above is required and is per-session — one approval covers this installation, not the next. The skill never types, pastes, invents, or requests credentials, API keys, or payment details into anything; when a step needs one it explains what the user must enter and where, then waits. Account creation, purchases, and terms acceptance are always the user's own steps, and host permission prompts are never bypassed.

### Verify like the user would

When the product has a graphical experience, done means that experience actually opened — load the page or launch the app and see its main screen — never just a command echoing a version. For command-line products, a version command answering is enough. Then report what was verified, leading with the one thing the user should do to start using it ("open http://localhost:3000 in your browser — you'll see the search box"). On failure, report the failing step and its output in plain words and hand over the manual for the rest; do not retry in a loop.
