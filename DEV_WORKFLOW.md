# Developer Workflow Reference

Personal reference for working with `ai-file-sorter` — covers SSH signing, the two-track git workflow, and how to contribute changes back to the upstream author.

---

## SSH Commit Signing

All commits on this repo are signed with an SSH key. This is a one-time setup — once done, signing is automatic.

### First-time setup (already done)

```powershell
# Configure git to use SSH for signing
git config --global gpg.format ssh
git config --global user.signingkey "C:\Users\mmche\.ssh\github_signing.pub"
git config --global commit.gpgsign true

# Point git at Windows system ssh-keygen so it uses the SSH Agent
git config --global gpg.ssh.program "C:\Windows\System32\OpenSSH\ssh-keygen.exe"

# Enable and start the Windows SSH Agent (requires Admin PowerShell)
Set-Service ssh-agent -StartupType Automatic
Start-Service ssh-agent

# Add your signing key to the agent (one time — survives reboots)
ssh-add C:\Users\mmche\.ssh\github_signing
```

### Use the noreply email to protect your real address

GitHub blocks pushes that expose your private email. Always use the noreply address:

```powershell
git config --global user.email "259145294+mmchenry76@users.noreply.github.com"
```

### Verify signing is working

```powershell
git config --global gpg.format       # should return: ssh
git config --global user.signingkey  # should return path to .pub file
git config --global commit.gpgsign   # should return: true
```

Commits will show a **Verified** badge on GitHub when signing is working correctly.

---

## Two-Track Workflow

This repo has personal security hardening applied (CodeQL, Dependabot, SHA-pinned Actions, SECURITY.md). This means your `main` branch **diverges from upstream** and should never be used as the base for upstream PRs.

### Track 1 — Your fork (personal use, experiments, local improvements)

Branch from **your** main:

```powershell
git checkout main
git checkout -b feature/my-experiment
# develop, commit, push
git push origin feature/my-experiment
# open PR to your fork only
```

### Track 2 — Upstream contributions (clean PRs to the original author)

Branch from **upstream** main — your PR contains only your change, none of the hardening:

```powershell
git fetch upstream
git checkout -b fix/my-bugfix upstream/main
# make your changes
git push origin fix/my-bugfix
# open PR to hyperfield/ai-file-sorter (not your fork)
```

---

## Remote Setup (one-time)

```powershell
# Add upstream if not already present
git remote add upstream https://github.com/hyperfield/ai-file-sorter.git

# Verify both remotes
git remote -v
# origin    https://github.com/mmchenry76/ai-file-sorter.git
# upstream  https://github.com/hyperfield/ai-file-sorter.git
```

---

## Keeping Your Fork Up to Date with Upstream

```powershell
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Security Hardening in This Repo

| Layer | File | Notes |
|---|---|---|
| CodeQL scanning | `.github/workflows/codeql.yml` | Runs on every PR and weekly |
| Dependabot | `.github/dependabot.yml` | Weekly checks for Actions + pip deps |
| SHA-pinned Actions | `.github/workflows/build.yml` | Actions pinned to full commit SHAs |
| Security policy | `SECURITY.md` | Vulnerability disclosure process |
| Branch protection | GitHub settings | PRs required on main |
| Secret scanning | GitHub settings | Push protection enabled |
| SSH commit signing | git config + SSH Agent | Verified badge on every commit |
| Vigilant mode | GitHub settings | Unsigned commits flagged |

---

## Common Commands Quick Reference

```powershell
# Start a new personal feature
git checkout main && git pull && git checkout -b feature/name

# Start a clean upstream contribution
git fetch upstream && git checkout -b fix/name upstream/main

# Commit (signing is automatic)
git add .
git commit -m "type: description"

# Push and open PR
git push origin branch-name

# Sync fork with upstream
git fetch upstream && git checkout main && git merge upstream/main && git push origin main

# Clean up merged branch
git checkout main && git branch -d branch-name
```

---

## VS Code Tips

- **Workspace Trust:** `Ctrl+Shift+P` → Manage Workspace Trust → Trust (required once per folder)
- **Copilot Chat:** Use `@workspace` to give Copilot full codebase context
- **Source Control panel:** `Ctrl+Shift+G` — commit, push, branch all without leaving VS Code
