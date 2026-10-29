# Service repositories

Extra rules for `dumpen`, `jobb`, `klarsprak` and `politiker`: Cloudflare Workers services with a Docker image for self-hosting. They share the pattern below. Everything else is in [GOVERNANCE.md](GOVERNANCE.md).

## Placeholders and deploy

- The code is generic and contains only `__NAME__` placeholders. Declare every placeholder in use in `cloudflare.example.env` with a sample value and a comment on where the real value comes from.
- `node tools/render.mjs <values.env | -> <outdir> [overlay]` copies the repository, fills in every declared placeholder (stops if one is missing), lays the overlay on top, removes routes with an empty pattern and links `node_modules`. With `-` the values come from environment variables.
- Production is deployed by Workers Builds when a pull request is merged to `main`: render with `node tools/render.mjs - build`, then `npx wrangler deploy --cwd build` (the repository's `AGENTS.md` names the directory if it differs). Only `main` is built. Self-deploy works the same way from a private values file; add secrets with `npx wrangler secret put`.
- Deploy the new Worker before a migration that drops tables or columns. Never edit a migration that has run; add a new one.
- Tests run on the unrendered source and see the placeholders.

## Cost and logging

- Aim for Workers Free, 10 ms CPU per request. No full inventory or rescan per cron run, request or added object; work incrementally and store derived data when it is written.
- Logs: sampling 1, invocation logs on, traces 0.01, query strings masked, Workers Issues on. Log errors and warnings, at most one or two lines per run. Handled errors reach Issues only through `console.error`.

## Documentation

`README.md`, `docker/README.md` and `docs/` are also the documentation site on GitHub Pages, built with Zensical (`zensical.toml`, `requirements-docs.txt`) and published from `main`.

- Build locally: `pip install -r requirements-docs.txt` (preferably in a virtual environment), then `node tools/docs-site.mjs && zensical build` (output in `site/`).
- The wiki is mirrored one way from the repository. Never edit it.
- `README.md` keeps its `**Documentation:**` line at the top. New pages under `docs/` go into `nav` in `zensical.toml`; their titles become the wiki page names.
- `jobb`, `klarsprak` and `politiker` are documented in Swedish. `docs/eng/` mirrors the documentation in English (`README.md` is `docs/eng/index.md`, `docker/README.md` is `docs/eng/docker.md`, other pages keep their names). Update both in the same pull request and add new pages to both `nav` sections. Everything else is English.

## Docker and release

- Files: `Dockerfile`, `docker-compose.yml`, `CHANGELOG.md` and `docker/` (`README.md` is the Docker Hub page, plus `entrypoint.sh`, `import-settings.sh` and the container's wrangler configuration).
- The Worker runs in workerd through `wrangler dev --local` with all state under `/data`. The entrypoint renders the source from the container's environment with working defaults and generates missing encryption keys once; existing keys are never overwritten. The image starts empty and never reads from anyone's Cloudflare resources.
- Settings come from `docker-compose.yml`, or once from `/data/import.env`, which `import-settings.sh` merges into `/data/settings.env` and deletes. Non-empty compose variables win.
- `docker/README.md`, `docker-compose.yml` and `CHANGELOG.md` must agree. The documentation names the Docker Hub image to use in compose instead of building from the repository.
- Release: bump the version (the file named in the repository's `AGENTS.md`) by semver and add `## X.Y.Z – YYYY-MM-DD` to `CHANGELOG.md`. On merge, `release.yml` creates the tag and release and calls `docker.yml`, which publishes `X.Y.Z`, `X.Y`, `X` and `latest`. A merge without a version bump releases nothing.
- If the Docker job fails, re-run the failed jobs in the release run. If the release stopped after the tag was created, run `docker.yml` manually (workflow_dispatch on the tag). If the session lacks permission, ask the owner.

## Review

CodeRabbit inherits the central configuration in `pissmyran/coderabbit`. The repository's `.coderabbit.yaml` lists risk areas with review instructions; read the part for the files you change before starting.
