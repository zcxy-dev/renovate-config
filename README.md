# renovate-config

The Renovate preset that zcxy-dev repositories extend with `"extends": ["github>zcxy-dev/renovate-config"]` in their `.github/renovate.json`. Every repository extends it except HTT (another owner's code) and htt-payroll (keeps Dependabot until it retires). This README is the one description of the preset; `default.json` is the preset itself, and its `description` fields only label each rule.

It is public so Renovate can read it from any repository, whatever that repository's visibility. Change `default.json` by pull request; the `validate` check runs `renovate-config-validator --strict --no-global` on it and on this repository's own `.github/renovate.json`, on a GitHub-hosted runner.

## What the preset does

Renovate updates every extending repository's dependencies; Dependabot only raises alerts.

- **Base.** `config:recommended`, so it covers npm and pnpm, Python (`pyproject.toml`, `requirements*.txt`, `uv.lock`), mise, `.node-version` and `.python-version`, Dockerfiles and GitHub Actions, with every action pinned to a full commit SHA and its version in a comment. Commits use semantic messages with the scope `deps`, and every pull request gets the `dependencies` label. No dependency dashboard issue is opened.
- **Schedule.** Updates open Mondays 00:00-03:59 Asia/Taipei, so they don't compete with daytime work for the self-hosted runner. Security updates and the shared-workflows pins open at any time.
- **Seven days old.** A release is offered only once it is seven days old (`minimumReleaseAge`), so a compromised release is usually pulled before it reaches a repository.
- **Grouped.** One pull request each for the npm, Python, mise, Docker and GitHub Actions minor and patch updates; a major update gets its own pull request.
- **Node.** Node moves in one pull request wherever it is named (the Docker image, `.node-version`, mise, `engines`), so files that must match never land apart. It skips the seven-day wait, since Docker Hub dates an image tag by its last rebuild. A Node major still waits wherever the repository's own rule holds it back.
- **Shared workflows.** The pins of `zcxy-dev/dotfiles/.github/workflows/<name>.yml@<sha> # workflows-vX.Y.Z` move together to each new `workflows-v` release, without the seven-day wait, since the releases are our own.
- **Merged on its own.** Minor, patch, pin and digest updates (`pinDigest` included) merge through GitHub's auto-merge once the required checks pass; a major update waits for a person.
- **Security updates.** Renovate turns each Dependabot alert into a pull request at once, labelled `security`, without the seven-day wait. Dependabot security updates are off in every extending repository, so the two never open the same update.

## Using it in a repository

`.github/renovate.json` is `{"extends": ["github>zcxy-dev/renovate-config"]}`, plus only the repository's own additions, each with its reason: a held-back update, or a manager the base does not set up right for it (requirements files compiled with hashes by `uv pip compile` need the `pip-compile` manager, with `pip_requirements` off for them). Validate it with the validator version `.github/workflows/validate.yml` pins: `npx --yes --package renovate@<version> -- renovate-config-validator --strict --no-global .github/renovate.json`.
