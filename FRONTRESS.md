# Bugsink for Team Frontress

The `frontress` branch is upstream Bugsink (from the tag it is based on) plus
the fixes we need before they land upstream. Pushing to it builds
`ghcr.io/gru2007/bugsink:frontress` (see `.github/workflows/frontress-image.yml`).

## Fixes

- **Windows minidumps were dropped.** PE modules carry the PDB age after the
  UUID in their debug id (`<uuid>-1`). `files/minidump.py` passed that straight
  to `UUID()`, which raised during digest, after ingest had already answered
  200, so the event was lost without a trace. The age is now dropped
  (`debug_id_to_uuid`).

## Updating to a new upstream version

    git fetch --tags https://github.com/bugsink/bugsink
    git rebase --onto <new tag> <old tag> frontress
    git push --force-with-lease origin frontress

Drop a fix from this branch once upstream has it.
