---
description: Stage all changes and create commit (use with caution)
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*), Bash(git log:*)
---

# Commit Everything

⚠️ **CAUTION**: Stage ALL changes and commit. Use only when confident all changes belong together.

## Workflow

### 1. Analyze Changes

Run in parallel:

- `git status` - Show modified/added/deleted/untracked files
- `git diff --stat` - Show change statistics
- `git log -1 --oneline` - Show recent commit for message style

### 2. Safety Checks

**❌ STOP and WARN if detected:**

- Secrets: `.env*`, `*.key`, `*.pem`, `credentials.json`, `secrets.yaml`, `id_rsa`, `*.p12`, `*.pfx`, `*.cer`
- API Keys: Any `*_API_KEY`, `*_SECRET`, `*_TOKEN` variables with real values (not placeholders like `your-api-key`, `xxx`, `placeholder`)
- Large files: `>10MB` without Git LFS
- Build artifacts: `node_modules/`, `dist/`, `build/`, `__pycache__/`, `*.pyc`, `.venv/`
- Temp files: `.DS_Store`, `thumbs.db`, `*.swp`, `*.tmp`

**API Key Validation:**
Check modified files for patterns like:

```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ Real key detected!
AWS_SECRET_KEY=AKIA...         # ❌ Real key detected!
STRIPE_API_KEY=sk_live_...    # ❌ Real key detected!

# ✅ Acceptable placeholders:
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ Verify:**

- `.gitignore` properly configured
- No merge conflicts
- API keys are placeholders only

### 3. Request Confirmation

Present summary:

```
📊 Changes Summary:
- X files modified, Y added, Z deleted
- Total: +AAA insertions, -BBB deletions

🔒 Safety: ✅ No secrets | ✅ No large files | ⚠️ [warnings]
🌿 Branch: [name]

I will: git add . → commit

Type 'yes' to proceed or 'no' to cancel.
```

**WAIT for explicit "yes" before proceeding.**

### 4. Execute (After Confirmation)

Run sequentially:

```bash
git add .
git status  # Verify staging
```

### 5. Generate Commit Message

Analyze changes and create conventional commit:

**Format:**

```
[type]: Brief summary (max 72 characters)

- Key change 1
- Key change 2
- Key change 3
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`

**Example:**

```
docs: Update concept README files with comprehensive documentation

- Add architecture diagrams and tables
- Include practical examples
- Expand best practices sections
```

### 6. Commit

```bash
git commit -m "$(cat <<'EOF'
[Generated commit message]
EOF
)"
git log -1 --oneline --decorate  # Verify
```

### 7. Confirm Success

```
✅ Successfully committed!

Commit: [hash] [message]
Branch: [branch]
Files changed: X (+insertions, -deletions)
```

## Error Handling

- **git add fails**: Check permissions, locked files, verify repo initialized
- **git commit fails**: Fix pre-commit hooks, check git config (user.name/email)

## When to Use

✅ **Good:**

- Multi-file documentation updates
- Feature with tests and docs
- Bug fixes across files
- Project-wide formatting/refactoring
- Configuration changes

❌ **Avoid:**

- Uncertain what's being committed
- Contains secrets/sensitive data
- Merge conflicts present
- Want granular commit history
- Pre-commit hooks failing

## Alternatives

If user wants control, suggest:

1. **Selective staging**: Review/stage specific files
2. **Interactive staging**: `git add -p` for patch selection

**⚠️ Remember**: Always review changes before committing. When in doubt, use individual git commands for more control.
