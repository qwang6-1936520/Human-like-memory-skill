# Hermes Publish Runbook

Repository target: `qwang6-1936520/humanlike-memory-skill`

## 1. Create GitHub Repository

Create an empty repository in your GitHub account:

- Owner: `qwang6-1936520`
- Name: `humanlike-memory-skill`
- Visibility: your choice
- Do not initialize with README/license/gitignore

## 2. Push Local Repository

```bash
cd "/Users/wqapie/Desktop/Side Project/Fanren_20260315/humanlike-memory-skill"
git remote add origin git@github.com:qwang6-1936520/humanlike-memory-skill.git
git push -u origin main
```

If using HTTPS:

```bash
git remote add origin https://github.com/qwang6-1936520/humanlike-memory-skill.git
git push -u origin main
```

## 3. Install Hermes CLI

Use the official installer:

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

Then verify:

```bash
hermes --version
```

## 4. Authenticate

Complete required interactive authentication in your local shell:

```bash
hermes setup
```

## 5. Publish Skill

From repository root:

```bash
hermes skills publish . --to github --repo qwang6-1936520/humanlike-memory-skill
```

## 6. Verify Installability

```bash
hermes skills search human-like-memory
hermes skills inspect github:qwang6-1936520/humanlike-memory-skill
hermes skills install github:qwang6-1936520/humanlike-memory-skill
```
