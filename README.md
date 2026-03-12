# openclaw-pm-skills

Private repository of production-ready OpenClaw skills for PM / product / analytics workflows.

This repository is written **for AI agents first**. The goal is simple:
- discover the right skill quickly
- install it with minimal human help
- avoid permission/setup mistakes
- reach a working state with **0 avoidable errors**

---

## Installation model

An AI that supports skills should treat this repository as a **source-of-truth skill pack**.

### Preferred install path

If ClawHub install from Git is supported in the current environment, install from this repository directly.

Otherwise:
1. clone this repository locally
2. copy the desired skill folder into the OpenClaw `skills/` directory
3. follow the skill's `SKILL.md`

---

## Repository structure

```text
openclaw-pm-skills/
├── README.md
└── skills/
    └── ga4-data-api/
        ├── SKILL.md
        ├── references/
        └── scripts/
```

Each folder under `skills/` is a self-contained skill.

---

## Skills

### 1. `google-analytics-4`

Use when an agent needs to:
- connect Google Analytics 4 to OpenClaw
- set up OAuth-first GA4 access
- query GA4 metrics such as `activeUsers`, `sessions`, `country`, `sessionSourceMedium`
- analyze trends, conversion events, source/country breakdowns

Location:

```text
skills/google-analytics-4
```

Entry file:

```text
skills/google-analytics-4/SKILL.md
```

---


### 2. `gog`

Use when an agent needs to:
- connect Google Workspace accounts
- read Gmail, Calendar, Drive, Docs, Sheets, Contacts
- build email/calendar workflows in OpenClaw
- authorize one or more Google accounts for work automation

Location:

```text
skills/gog
```

Entry file:

```text
skills/gog/SKILL.md
```

---

## Zero-error operating rules

Before an AI installs or uses any skill from this repo:

1. **Read the target skill's `SKILL.md` first**
2. **Check permissions before setup**
   - especially for enterprise/internal systems like GA4, Gmail, analytics, cloud APIs
3. **Check required accounts before setup**
   - do not assume service accounts are needed
   - do not assume the currently logged-in Google account has access
4. **Prefer the simplest working path first**
   - OAuth user auth before service accounts
   - local validation before automation
5. **Be honest about blockers**
   - if a permission button is missing, call it a permission blocker
   - if an OAuth app is in testing, call out test-user setup
   - do not invent workarounds

---

## Recommended install workflow for AI agents

### Option A — copy a skill into an OpenClaw workspace

```bash
git clone https://github.com/nanaco666/openclaw-pm-skills.git
mkdir -p ~/.openclaw/workspace/skills
cp -R openclaw-pm-skills/skills/google-analytics-4 ~/.openclaw/workspace/skills/
```

Then read:

```text
~/.openclaw/workspace/skills/google-analytics-4/SKILL.md
```

### Option B — use the repo as a reference pack

If direct install is not appropriate, keep the repo cloned and load only the needed skill folder.

---

## Validation workflow

After installing a skill, an AI should always validate the smallest useful path.

For `google-analytics-4`, the minimum validation is:

```bash
bash ~/.openclaw/workspace/skills/google-analytics-4/scripts/install_ga4_openclaw.sh <GA4_PROPERTY_ID> <PATH_TO_CLIENT_SECRET_JSON>

python3 ~/.openclaw/workspace/skills/google-analytics-4/scripts/ga4_query.py \
  --metrics activeUsers,sessions \
  --dimensions date \
  --start 7daysAgo \
  --end today \
  --pretty
```

A valid result is a JSON response with metric rows, not just a successful OAuth prompt.

---

## Publishing standard for future skills

A new skill added to this repo should include:
- `SKILL.md`
- only the scripts/references actually needed
- explicit setup prerequisites
- explicit verification command
- explicit known pitfalls

Avoid fluffy README-style prose inside skills. Keep the repository and each skill operational.

---

## Current status

- Repository visibility: **private**
- Intended audience: trusted collaborators with access
- First valid skill: **`google-analytics-4`**
