---
name: tapdb-connector
description: Connect OpenClaw to TapDB project data for game analytics, including source, retention, revenue, lifecycle, and whale-user queries. Use when a Tap/TapDB game developer wants to pull data from TapDB, set up MCP keys, validate project access, or analyze project metrics by region, source, retention, revenue, or lifecycle. This skill is only relevant for Tap game developers / teams with valid TapDB access.
---

# TapDB Connector

Use this skill to set up and query TapDB through MCP keys.

## Scope

This skill is **only for Tap game developers with TapDB access**.

If the user does not have TapDB access, project access, or MCP keys, stop and say so plainly.

## Default approach

Do not scrape the TapDB web UI.

Use the provided query script and MCP keys.

## Hard rules

1. **Verify access first**
   - Ask whether the user can log into TapDB.
   - Ask whether they already have `TAPDB_MCP_KEY_CN` / `TAPDB_MCP_KEY_SG`.
   - Ask which region/project they actually want.

2. **Do not ask users to paste secrets in chat unless absolutely necessary**
   - Prefer writing the key locally via shell config.
   - If a secret is pasted, do not echo it back in full.

3. **Validate the smallest path first**
   - First run `list_projects`.
   - Only after project access is confirmed, run real reports.

4. **Be honest about environment blockers**
   - If SSL verification fails on macOS Python, call it out and set `SSL_CERT_FILE`.
   - If the key is region-specific, say so.
   - If the project list does not contain the target project, stop instead of guessing IDs.

## Setup flow

### Step 1 — Configure key

Write the correct region key into shell config.

Examples:

```bash
export TAPDB_MCP_KEY_SG="..."
export TAPDB_MCP_KEY_CN="..."
```

Persist it in `~/.zshrc` or `~/.bashrc`.

### Step 2 — Fix SSL if needed

On some macOS Python environments, HTTPS requests fail due to certificate verification.

If that happens, export:

```bash
export SSL_CERT_FILE="$(python3 -c 'import certifi; print(certifi.where())')"
```

Persist it in shell config if the environment needs it.

### Step 3 — Validate access

Run the smallest useful check first:

```bash
python3 ~/.openclaw/workspace/skills/tapdb-connector/scripts/tapdb_query.py -r sg list_projects
```

Switch `-r cn` or `-r sg` based on the actual key/project region.

### Step 4 — Query real data

Examples:

Yesterday source by country:

```bash
python3 ~/.openclaw/workspace/skills/tapdb-connector/scripts/tapdb_query.py \
  -r sg source -p <PROJECT_ID> -s 2026-03-11 -e 2026-03-11 \
  -g activation_country --group-dim cy --language en
```

Yesterday income:

```bash
python3 ~/.openclaw/workspace/skills/tapdb-connector/scripts/tapdb_query.py \
  -r sg income -p <PROJECT_ID> -s 2026-03-11 -e 2026-03-11 -g time --group-unit day
```

## Analysis guidance

- Separate **traffic/acquisition**, **retention**, and **revenue** instead of mixing them in one conclusion.
- Small-sample country rows should be called out as noisy.
- If there is no payment, say so directly instead of stretching the interpretation.
- If the user asks for a metric not exposed by the script/API, say it is not available and explain the closest available metric.

## References

- Read `references/setup.md` for the shareable setup process.
- Use `scripts/tapdb_query.py` directly instead of rewriting API calls from scratch.
