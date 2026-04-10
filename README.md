# create-agent-first-wiki

A reusable starter kit for building a local-first, agent-operated wiki with LLM-based routing.

This repository packages the skill definition plus starter scripts and templates needed to bootstrap a wiki that works like this:

capture → raw entry → route → absorb → maintain → automate

## What it includes

- `SKILL.md` — reusable skill definition for Hermes or any agent workflow
- `scripts/bootstrap_wiki.py` — creates the wiki tree and copies starter files into the new wiki
- `scripts/bootstrap_router.py` — creates the initial router config
- `scripts/write_entry.py` — writes raw entries into the wiki
- `scripts/absorb.py` — routes and absorbs pending entries into `mind/` and `world/`
- `scripts/rebuild_index.py` — rebuilds `meta/INDEX.md`
- `scripts/rebuild_backlinks.py` — rebuilds `meta/BACKLINKS.json`
- `scripts/cleanup.py` — runs basic integrity checks and writes a cleanup report
- `scripts/run_daily.py` — runs the daily maintenance cycle
- `scripts/run_weekly.py` — runs the weekly maintenance cycle
- `scripts/breakdown.py` — generates a lightweight weekly breakdown report
- `templates/` — starter templates for raw entries, wiki pages, and router prompts
- `references/example-tree.md` — example wiki structure

## Core ideas

- local-first canonical storage
- every input becomes a raw entry before interpretation
- `mind/` and `world/` are separate
- LLM routing is part of the base architecture
- routing failures stay visible instead of being hidden by silent fallback behavior
- index, backlinks, and logs are rebuildable from filesystem state

## Expected wiki layout

```text
wiki/
  meta/
  inbox/
  raw/
  mind/
  world/
  archive/
  scripts/
  templates/
```

See `references/example-tree.md` for the fuller structure.

## Quick start

### 1. Bootstrap the wiki

```bash
python3 scripts/bootstrap_wiki.py /path/to/wiki
```

That creates the folder tree and copies the starter templates and scripts into `/path/to/wiki`.

### 2. Bootstrap the router

```bash
python3 /path/to/wiki/scripts/bootstrap_router.py /path/to/wiki
```

### 3. Configure routing

Edit:

```text
/path/to/wiki/meta/ROUTER/router_config.json
```

Set at least:

- `model`
- `api_key_env`
- `base_url` if needed for your provider

Export the API key expected by `api_key_env`.

### 4. Create a test raw entry

```bash
python3 /path/to/wiki/scripts/write_entry.py /path/to/wiki \
  --title "First entry" \
  --text "A test note for the agent-first wiki."
```

### 5. Run absorb

```bash
python3 /path/to/wiki/scripts/absorb.py /path/to/wiki
```

### 6. Run the daily cycle

```bash
python3 /path/to/wiki/scripts/run_daily.py /path/to/wiki
```

## Failure behavior

If routing fails because of provider errors, missing API keys, invalid JSON, or validation failures:

- the failure is written to `meta/ROUTER/errors/`
- the raw entry moves to `inbox/manual-review/` or `inbox/failed/`
- cleanup and status scripts reflect that state

That behavior is deliberate. The kit avoids silent fallback that would hide routing problems.

## Who this is for

Use this if you want:

- an agent-operated personal wiki
- minimal manual filing
- a reusable starting point instead of inventing the workflow from scratch

## License

MIT
