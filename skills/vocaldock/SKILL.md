---
name: vocaldock
description: Process audio with VocalDock from the terminal. Use when the user wants to remove or isolate vocals from a song or video (acapella, karaoke/instrumental), split a song into stems or isolate an instrument (drums, bass, guitar, piano), remove background noise from a voice recording, turn text into speech or a voiceover, or generate a song or background music from a description.
---

# VocalDock

VocalDock runs AI audio tasks on the user's VocalDock account through a command-line tool.
Every command is run with `npx -y vocaldock@latest <command>`; no install step is needed (Node.js 20+).

## What it can do

| User wants to | Command |
|---|---|
| Remove vocals / get an acapella or karaoke track from a song or video | `separate <file-or-url>` |
| Split a song into stems, or isolate/remove one instrument (drums, bass, guitar, piano) | `stems <file-or-url>` (add `--only drums,bass` to download just those) |
| Clean up background noise in speech (podcast, voice memo, interview) | `denoise <file-or-url>` |
| Turn text into speech / a voiceover | `voices` to pick a voice (`--mine` for the user's cloned voices), then `tts --voice <id> --text "..."` |
| Generate a song or background music | `music --prompt "..."` (add `--instrumental` for no vocals) |
| Find or re-download an earlier result (also from the website or app) | `history`, then `<command> --task <id>` |
| Check the account and credit balance | `whoami` |

Inputs can be a local file path or a public http(s) URL. Results are downloaded into the current
directory (or `-o <dir>`). Before using a command for the first time, run it with `--help` to see
its options.

## Signing in

If a command says the user is not signed in:

1. Run `npx -y vocaldock@latest login --no-wait --json` and show the `verification_url` to the user.
   Ask them to open it, check the code matches `user_code`, and click Authorize.
2. After the user confirms, run `npx -y vocaldock@latest login --continue`.

In a cloud environment, the user can instead create a key at https://vocaldock.com/dashboard/api-keys
and set it as the `VOCALDOCK_API_KEY` environment variable.

## Running tasks

- Add `--json` to get a single JSON object on stdout with the saved file paths and credits used.
  Progress messages go to stderr.
- Tasks take from about 30 seconds (vocal separation) to 3 minutes (music). If your command timeout
  is short, add `--no-wait` and later run the `resume` command it prints (`<command> --task <id>`).
- Errors start with what to do next (sign in, top up credits, fix the file path). Relay them to the user.
- Tasks spend the user's credits. Run one task at a time unless the user asked for several.
- In sandboxed agents, the first run may need the user to allow network access.
