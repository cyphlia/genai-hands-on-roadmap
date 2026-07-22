# Setup Guide — From Zero to Your First Commit in VS Code

This guide assumes nothing is installed yet. Skip any step you've already done. Every command is meant to be typed into VS Code's built-in terminal (`` Ctrl+` `` on Windows/Linux, `` Cmd+` `` on Mac opens it).

---

## Part 1 — One-time tool installation

### 1. Install Git
- **Windows:** download from [git-scm.com](https://git-scm.com/download/win), run the installer, accept defaults.
- **Mac:** open Terminal and run `git --version` — macOS will prompt to install Xcode Command Line Tools if it's missing; accept.
- **Linux:** `sudo apt install git` (Debian/Ubuntu) or your distro's equivalent.

Verify it worked:
```bash
git --version
```
You should see something like `git version 2.4x.x`.

### 2. Install VS Code
Download from [code.visualstudio.com](https://code.visualstudio.com/) if you don't already have it.

### 3. Configure Git with your identity (one-time, ever)
```bash
git config --global user.name "cyphlia"
git config --global user.email "your-github-email@example.com"
```
Use the same email your GitHub account (`@cyphlia`) is registered with.

### 4. (Recommended) Install the GitHub CLI — makes repo creation a single command
- **Windows:** `winget install --id GitHub.cli`
- **Mac:** `brew install gh`
- **Linux:** see [cli.github.com](https://cli.github.com/) for your package manager

Then authenticate once:
```bash
gh auth login
```
Follow the prompts — choose GitHub.com, HTTPS, and log in via browser. This lets you create/push repos from the terminal without pasting tokens manually.

*(If you'd rather not install the CLI, Part 2 has a no-CLI alternative using the GitHub website.)*

---

## Part 2 — Create the GitHub repository

### Option A — Using GitHub CLI (fastest)

From the folder containing the `genai-hands-on-roadmap` project (the one with `README.md`, `ROADMAP.md`, and the `phaseX-...` folders):

```bash
cd path/to/genai-hands-on-roadmap
git init
git add .
git commit -m "chore: initial commit — roadmap, checklist, and phase scaffolding"
gh repo create cyphlia/genai-hands-on-roadmap --public --source=. --remote=origin --push
```

That single `gh repo create` command creates the repo under your account, wires up the remote, and pushes in one go. Done — skip to Part 3.

### Option B — Using the GitHub website (no CLI)

1. Go to [github.com/new](https://github.com/new) while logged in as `@cyphlia`.
2. Repository name: `genai-hands-on-roadmap` (or any name you prefer).
3. Keep it **Public** (so you can link it/show progress) or **Private** — your call.
4. **Do not** check "Add a README" — you already have one locally, and this avoids a merge conflict on first push.
5. Click **Create repository**. GitHub will show you a page with setup commands — ignore the "…or create a new repository on the command line" block (you already have files) and use the one under **"…or push an existing repository from the command line."**

Back in your VS Code terminal, inside the project folder:
```bash
cd path/to/genai-hands-on-roadmap
git init
git add .
git commit -m "chore: initial commit — roadmap, checklist, and phase scaffolding"
git branch -M main
git remote add origin https://github.com/cyphlia/genai-hands-on-roadmap.git
git push -u origin main
```

The first push may open a browser window asking you to authorize VS Code/Git to access your GitHub account — approve it.

---

## Part 3 — Open the repo in VS Code properly

If you didn't build the folder inside VS Code already:

```bash
code path/to/genai-hands-on-roadmap
```
(If `code` isn't recognized as a command, open VS Code, press `Cmd/Ctrl+Shift+P`, type "Shell Command: Install 'code' command in PATH", and run it — then retry.)

Once open, check the **Source Control** icon in the left sidebar (looks like a branching icon). This is where you'll see changed files, stage them, and commit — as an alternative to typing `git add`/`git commit` by hand, if you prefer clicking.

---

## Part 4 — The Commit Habit (this is the part that actually matters)

A repo with a good structure that never gets touched again is just a folder. The goal is to make committing a **default reflex**, not a decision you have to motivate yourself into each time.

### The core loop, every session
1. Open a project folder inside the relevant `phaseX-.../` directory and write code.
2. When you get something working (even partially), **stop and commit immediately** — don't wait for "done."
3. If you finished or made progress on a checklist item, tick the checkbox in that phase's `README.md` **and** the root `README.md`, and include that in the same commit.
4. Add a line to `STUDY_LOG.md` before you close your laptop — even a two-line entry.

### Exact commands for step 2-3
```bash
git add .
git commit -m "feat(phase1): tokenizer encodes/decodes round-trip correctly"
git push
```

### Commit message convention (keeps your history actually useful later)
Prefix with what kind of change it is:
- `feat:` — new working functionality (e.g., `feat(phase5): RAG pipeline returns cited answers`)
- `fix:` — bug fix (e.g., `fix(phase2): retry loop wasn't respecting max attempts`)
- `docs:` — README/checklist/log updates only
- `refactor:` — restructuring code without changing behavior
- `chore:` — setup, dependencies, config

This isn't strict — the point is that six months from now, `git log --oneline` tells a story you can actually read.

### Making "constant" commits realistic, not forced
You don't need a commit every day to make progress real — you need **no session without a commit**. A rule that works well:

> **Never close VS Code on a session where you touched code without running `git add . && git commit -m "..."` first — even if the code is broken.**

Committing broken/WIP code is fine and expected:
```bash
git commit -m "wip(phase6): agent loop parses tool calls but doesn't execute yet"
```
This matters more than a perfect commit history. A repo with 40 small honest commits teaches you (and shows anyone reviewing it) far more than one giant commit dropped in at the end.

### Turning checkboxes into a visible habit
- Every time you check a box in `README.md`, that's its own tiny commit: `docs: check off project 3a`. Small, satisfying, and it keeps the root README an honest live status of the whole roadmap.
- GitHub renders `- [ ]` / `- [x]` checkboxes as actual clickable checkboxes when viewing the file on github.com — so your progress is visible at a glance on your repo's homepage without opening any file.
- Optional: turn on your GitHub contribution graph as a visual nudge — it's on your profile automatically once you're pushing regularly to a repo under your account. Don't chase it artificially (empty commits just to fill a square defeats the purpose) — let it reflect real sessions.

### If you miss a stretch of days
Don't create a "catch-up" mega-commit that squashes a week of work into one vague message. Instead, when you sit back down, treat it exactly like a fresh session: small commits, one per working change, starting from wherever you left off. Consistency resuming matters more than an unbroken streak.

---

## Part 5 — Day-to-day workflow once you're set up

```bash
# start of a session — make sure you're in sync
git pull

# ...write code in phaseX-.../ folder...

# whenever something works (or you're stopping for the day)
git add .
git commit -m "feat(phaseX): <what you just made work>"
git push
```

That's the entire loop, repeated for months. The roadmap in `ROADMAP.md` tells you *what* to build each week; this file's Part 4 is *how* you make sure it actually accumulates into a real, visible body of work instead of stalling after the first excited weekend.
