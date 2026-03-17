# Cursor: Use Shared Skills and Instructions via Symlinks

## Step 1 – Add `.cursor` in project or global

- **Project:** Use a `.cursor` folder **inside each repo** (e.g. `my-project/.cursor/`). Rules apply only when that project is open.
- **Global:** Use Cursor’s user directory **`~/.cursor/`** (e.g. on macOS `~/.cursor/skills/`). Things here apply in every project.

Choose one:
- **Project** → create `.cursor` in the repo (instructions/rules per project).
- **Global** → use `~/.cursor/` (same instructions/skills for all projects).

---

## Step 2 – Clone the shared repository

Clone your shared repo (e.g. `fusion-ai-collection`) somewhere stable on your machine so you can symlink to it in Step 3.

```bash
# Example: clone into a workspace folder (adjust URL and path to your setup)
git clone <URL-of-your-shared-repo> /<user>/Workspace/my-ai-collection
cd /<user>/Workspace/my-ai-collection
```

Use this path in the symlink commands below.

---

## Step 3 – Symlink

**If you chose project (Step 1):**

```bash
# From the project root where you want the rules
mkdir -p .cursor/rules
ln -s /path/to/my-ai-collection/instructions .cursor/rules/instructions
```

**If you chose global (Step 1):**

```bash
# Skills live in user Cursor dir; rules are per-project (see note below)
mkdir -p ~/.cursor/skills

# Symlink each skill you want
ln -s /path/to/my-ai-collection/skills/test-coverage-checker ~/.cursor/skills/test-coverage-checker
ln -s /path/to/my-ai-collection/skills/readme-writer ~/.cursor/skills/readme-writer
# … one ln -s per skill folder
```

**Typical combo:** Instructions → project `.cursor/rules/` (symlink in each repo). Skills → global `~/.cursor/skills/` (symlink once per machine).

Replace `/path/to/my-ai-collection` with the path from Step 2 (e.g. `/<user>/Workspace/my-ai-collection`).

---

**Note:** Cursor loads **rules** from the open project’s `.cursor/rules/`. So “global” in Step 1 usually means “global **skills** in `~/.cursor/skills/`” and “project **rules** in each repo’s `.cursor/rules/`” via symlinks as in Step 3.
