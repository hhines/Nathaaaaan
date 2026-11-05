# Daily newsjack monitor — So Danca

A claude.ai scheduled task runs this every day at ~7am America/New_York in a fresh
cloud session on this repo. Each run:

1. Install newsjack (the cloud container is fresh every time):
   `NEWSJACK_RUN_SETUP=0 NEWSJACK_RUNTIMES=claude sh -c "$(curl -fsSL https://raw.githubusercontent.com/elvisun/newsjack/main/install.sh)"`
2. Load this monitor from the repo:
   `~/.newsjack/bin/newsjack monitor init so-danca --profile newsjack/profile.json --force`
   then copy `newsjack/brief.md` over `~/.newsjack/monitors/so-danca/brief.md`.
3. Restore seen-state so stories already reported are suppressed: copy the
   contents of `newsjack/state/` (if present) into `~/.newsjack/monitors/so-danca/`.
4. Run `~/.newsjack/bin/newsjack monitor run so-danca` and use the
   `newsjack-detector` skill to finish the analysis and write `run.md`.
5. Copy `run.md` to `newsjack/runs/<YYYY-MM-DD>.md`, save any seen-state files the
   CLI keeps in `~/.newsjack/monitors/so-danca/` (everything except `runs/`,
   `profile.json` and `brief.md`) back to `newsjack/state/`, commit and push to `master`.
6. Post a short summary (pitch-ready stories first, otherwise "nothing
   pitch-ready" plus 3 notable things found) to the configured Slack channel.

## Editing the monitor

- **Different news** → edit `profile.json` (`topics`, `search_terms`, `competitors`, `feed_urls`).
- **What gets pitched or shown** → edit `brief.md` (Audience, We pitch, We never pitch, How to surface).

## Network requirements

The cloud environment must allow: `raw.githubusercontent.com`, `github.com`,
`news.google.com`, plus any feed hosts added to `profile.json`. Optional:
`api.medialyst.ai` (better news search) and `api.x.com` (X News).
