# renovate-config

The shared Renovate preset. `README.md` is the one description of what the preset does and how a repository uses it; read it before changing `default.json`.

## Where things live

- `default.json`: the preset every extending repository loads from `main`.
- `.github/renovate.json`: this repository's own Renovate config; its custom manager raises the validator version in `.github/workflows/validate.yml`.
- `.github/workflows/validate.yml`: the `validate` check, on a GitHub-hosted runner because the repository is public.

## Validate

Run the `run:` line of `.github/workflows/validate.yml` from the repository root; it is the `validate` check's only step besides checkout.

## Conventions

- Merging to `main` is the release: every extending repository picks up a `default.json` change on its next Renovate run, with no tag, deploy or rollout step. Revert the PR to roll back.
- A change to what the preset does updates `README.md` in the same PR.
- Keep the validator invocation in `validate.yml` as `--package renovate@X.Y.Z`, the form the custom manager matches.

## Finish line

Done is the `validate` check green and the PR merged. There is no deploy and no `verify-deploy`.
