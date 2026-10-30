# .github

The shared home for every repository under `pissmyran`: common rules, default community files and the shared Renovate and CodeRabbit setup. The repository holds no code.

## What is here

| File | Purpose | Reaches other repositories |
| --- | --- | --- |
| [GOVERNANCE.md](GOVERNANCE.md) | Rules for every repository: branches, pull requests, review, security, releases. | Linked from each repository's `AGENTS.md` |
| [SERVICES.md](SERVICES.md) | Extra rules for `dumpen`, `jobb`, `klarsprak` and `politiker`: placeholders, deploy, Docker, documentation. | Linked from those repositories' `AGENTS.md` |
| [BASTION.md](BASTION.md) | Extra rules for `bastion` and the `bastion_*` repositories. | Linked from those repositories' `AGENTS.md` |
| [renovate-config.json](renovate-config.json) | Shared Renovate preset. | Repositories extend `local>pissmyran/.github:renovate-config` |
| [.github/FUNDING.yml](.github/FUNDING.yml) | Sponsor button. | Default for repositories without their own |
| [.github/ISSUE_TEMPLATE/](.github/ISSUE_TEMPLATE) | Bug report and feature request templates. | Default for repositories without their own |
| [.coderabbit.yaml](.coderabbit.yaml) | Review instructions for this repository; the base lives in `pissmyran/coderabbit`. | Only this repository |
| [LICENSE](LICENSE) | License of this repository. | No, each repository has its own |

## How the defaults work

GitHub treats a repository named `.github` as a source of fallback files. If another public repository has no file of the same kind, GitHub shows the one from here. A file in the repository itself always wins; the two are never merged.

- **Inherited as fallbacks:** issue and pull request templates, `FUNDING.yml`, and the community files `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md` and `GOVERNANCE.md`.
- **Not inherited:** `README.md`, `LICENSE`, workflows and `CODEOWNERS`. This README describes only this repository. Every other repository has its own `README.md`, and the two do not combine or compete.
- **Inherited by explicit reference, not by GitHub:** the three rule files are read through the links in each `AGENTS.md`, and Renovate through its `extends` line.

A repository that wants its own issue template must provide the whole set it needs: the fallback applies per kind of file, so one local template replaces the shared templates of that kind.

## CodeRabbit review coverage

[.coderabbit.yaml](.coderabbit.yaml) keeps `inheritance: true` to inherit the
base configuration from `pissmyran/coderabbit`. Central settings stay there;
this repository adds review instructions for the shared rule files, Renovate
preset, README, issue templates, funding configuration and CodeRabbit configuration.
`GOVERNANCE.md` and `**/AGENTS.md` are explicit code-guideline sources.
`SERVICES.md` and `BASTION.md` are review targets, not general guidelines here.
These local settings apply only to this repository and do not propagate to consumers.

[CodeRabbit inheritance](https://docs.coderabbit.ai/configuration/configuration-inheritance)
merges path instructions by their exact `path` key: a local entry replaces a
central entry with the same key, while distinct central entries remain.
After configuration changes, inspect the resolved configuration on a pull request:

```text
@coderabbitai configuration
```

Confirm that inherited review scope, security scanners and advisory checks remain
intact, and that automatic approvals and paid finishing touches remain disabled.
Schema validation alone cannot confirm these effective settings.

## Changing this repository

Changes to shared rules, presets or community defaults can affect consuming repositories, so weigh their scope carefully. Follow [GOVERNANCE.md](GOVERNANCE.md): own branch, squash-merged pull request, no personal values and no secrets.

- Rule files stay short and hold only what applies to more than one repository. Rules for a single repository belong in its `AGENTS.md`.
- Changes to `renovate-config.json` affect every dependency update; check them against the [Renovate schema](https://docs.renovatebot.com/renovate-schema.json).
