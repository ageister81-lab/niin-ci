# niin-ci

Shared CI/CD for every Niin repo (ADR 0008):

- `.github/workflows/node.yml`: the reusable workflow every repo calls. It covers typecheck, lint, format, tests, build, dependency audit, secret scan (TruffleHog), CodeQL, and, with `docker: true`, an image build plus Trivy scan.
- `renovate/default.json`: the shared Renovate preset (minimum release age, Action and image digests pinned, grouped updates).
- `templates/`: the source of truth for shared config (`eslint.config.js`, `tsconfig.json`, Prettier, editorconfig, gitignore). A change made here gets copied into each repo by PR; nothing syncs automatically yet.

**Read the platform docs first:** `../niin-infra/CLAUDE.md`, then `../niin-infra/docs/security.md` §9–10. They are binding.

## Repo rules

- Pin every third-party Action by full commit SHA with a `# vX.Y.Z` comment. Never use a tag or branch.
- Workflows default to `permissions: read-all`. Grant more per job, and only what the job needs.
- `actions/checkout` always uses `persist-credentials: false`.
- No long-lived cloud credentials. Deploy workflows (from M1) authenticate to GCP with Workload Identity Federation only.
- The `niin-signer` production deploy has a manual approval environment (EX-001 while solo).
