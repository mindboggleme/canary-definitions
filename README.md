# canary-definitions

Public, auto-refreshed signature/definition files for
[Canary-IDS](https://github.com/mindboggleme/Canary-IDS), the owner's
private Android intrusion-detection app.

## Why this repo exists

Four of Canary's detectors match against signatures compiled directly into
the APK, so a change (a vendor's firmware update, a new sample) could only
ever be fixed by a new Canary release the owner has to find and install --
unlike every other threat feed Canary uses, which already refreshes itself
daily from a public source (HaGeZi, abuse.ch, Spamhaus, and others).

Canary-IDS itself is a private GitHub repository. `raw.githubusercontent.com`
returns 404 for every file in a private repo unless the request carries a
GitHub auth token -- and embedding such a token in a distributed Android APK
would be a credential shipped inside every install, extractable by anyone
who decompiles it. That's an anti-pattern Canary has already declined once
for a different feature (its own in-app update check, CAN-189) for exactly
this reason, so it isn't reopened here either.

This repo exists to hold just the reviewed, non-sensitive definition data
in public, where `raw.githubusercontent.com` serves it with no credential
at all, while every other line of Canary's own source code stays private.

## What's here

- `flock_signatures.json` -- Flock Safety ALPR camera detection signatures
  (CAN-202, the pilot). See Canary-IDS's own
  `hardware/FlockSignatures.kt` and `hardware/FlockSignatureFeed.kt` for how
  this file is parsed, validated and used, and
  `docs/design/ALWAYS_UPDATING_DEFINITIONS.md` for the full design.

More files will be added here as the same pattern extends to Canary's other
compiled-in signature sets (`HackerSignatures`, `OffensiveToolSignatures`,
`MalwareStringSignatures`).

## Editing a definition file

Treat a change here with the same care as a change to Canary's own source:
read the corresponding Kotlin file first, bump the file's own `version`
field on every real change, and keep the commit message plain about what
changed and why. Canary falls back to its own compiled-in default on any
fetch or parse failure, so a mistake here degrades to "the old behavior,"
never a crash -- but it should still be treated as a real release, not a
scratch file.
