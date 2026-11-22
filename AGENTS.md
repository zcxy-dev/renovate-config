# renovate-config

The shared Renovate preset. `README.md` is the one description of what the preset does and how a repository uses it; read it before changing `default.json`, and keep it matching in the same PR.

This repository is public: keep the names of private repositories, hosts and people out of its files, commits and pull requests.

## Where things live

- `default.json`: the preset every extending repository loads from `main`.
- `.github/renovate.json`: this repository's own Renovate config, the preset alone. Renovate does not update the validator version in `.github/workflows/validate.yml`; raise it by hand when needed.

## Validate

Run the `run:` line of `.github/workflows/validate.yml` from the repository root.

## Release and rollback

Merging to `main` is the release: every extending repository picks up a `default.json` change on its next Renovate run, with no tag or deploy. Reverting the PR stops new updates under the bad rules; updates Renovate already merged in extending repositories need their own reverts there.

## Finish line

Done is the `validate` check green and the PR merged. For a `default.json` change, also confirm that after the next Renovate run no extending repository has an open "Action Required: Fix Renovate Configuration" issue (`gh search issues --owner zcxy-dev --state open "Fix Renovate Configuration"`).
