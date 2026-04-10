# Hermes Publish Runbook

Repository target: `qwang6-1936520/Human-like-memory-skill`

## 1. Create GitHub Repository

Create an empty repository in your GitHub account:

- Owner: `qwang6-1936520`
- Name: `Human-like-memory-skill`
- Visibility: your choice
- Do not initialize with README/license/gitignore

## 2. Push Local Repository

```bash
cd "/Users/wqapie/Desktop/Side Project/Fanren_20260315/humanlike-memory-skill"
git remote add origin git@github.com:qwang6-1936520/Human-like-memory-skill.git
git push -u origin main
```

If using HTTPS:

```bash
git remote add origin https://github.com/qwang6-1936520/Human-like-memory-skill.git
git push -u origin main
```

## 3. Install Hermes CLI

Install Hermes CLI using the official documentation for your OS, then verify:

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
hermes skills publish . --to github --repo qwang6-1936520/Human-like-memory-skill
```

## 6. Verify Installability

```bash
hermes skills search human-like-memory
hermes skills inspect github:qwang6-1936520/Human-like-memory-skill
hermes skills install github:qwang6-1936520/Human-like-memory-skill
```
