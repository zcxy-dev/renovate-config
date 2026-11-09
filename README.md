# renovate-config

The Renovate preset every zcxy-dev repository extends with `"extends": ["github>zcxy-dev/renovate-config"]` in its `.github/renovate.json`. It is public so Renovate can read it from any repository; change `default.json` by pull request, and the `validate` check runs `renovate-config-validator --strict` on it. Updates open Mondays 00:00-03:59 Asia/Taipei, so they don't compete with daytime work for the self-hosted runner; security updates and the shared-workflows pins open at any time.
