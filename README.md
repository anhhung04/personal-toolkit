# personal-toolkit

Personal [oh-my-pi](https://omp.sh) (omp) plugin marketplace by **anhhung04** — every plugin
in one catalog, including private repos pulled over SSH.

## Install the marketplace

```sh
omp plugin marketplace add git@github.com:anhhung04/personal-toolkit.git
```

Then browse and install:

```sh
omp plugin marketplace discover personal-toolkit
omp plugin install omp-docker@personal-toolkit
omp plugin install pi-vuln-hunting@personal-toolkit
```

Or in-session:

```
/marketplace add git@github.com:anhhung04/personal-toolkit.git
/marketplace discover personal-toolkit
/marketplace install omp-docker@personal-toolkit
```

## Prerequisites

Every entry clones over SSH (`git@github.com:…`), so your GitHub account needs the SSH key
for this machine added — including for the **public** repos, since the catalog always uses SSH.
Private entries additionally require the key be authorized for those repos.

Verify before installing:

```sh
ssh -T git@github.com
git ls-remote git@github.com:anhhung04/pi-vuln-hunting.git HEAD
```

Private keys must be mode `600` or `ssh` silently skips them and the clone fails with
`Permission denied (publickey)` — even for repos you reached fine from an interactive shell:

```sh
chmod 600 ~/.ssh/id_ed25519
```

## Plugins

| Plugin | Visibility | Description |
| --- | --- | --- |
| `omp-docker` | public | Native Docker tools — containers, images, builds, Compose, Swarm, sandboxes, networks, volumes, diagnostics, cleanup |
| `omp-iac` | public | IaC tools — OpenTofu/Terraform, Ansible, Pulumi, Floci local cloud emulation |
| `omp-ofx` | public | OFX red-team workflow engine — run/list/validate workflows, sessions, fleets, cloud profiles, config |
| `omp-memory-obsidian` | public | Obsidian-vault long-term memory — wiki topic pages, daily logs, prompt-time recall |
| `tracecat` | public | Tracecat MCP + Agent Skills — workflows, actions, cases, tables, agent presets, Slack bots, workspace skill uploads (port of [TracecatHQ/tracecat-plugins](https://github.com/TracecatHQ/tracecat-plugins)) |
| `pi-vuln-hunting` | **private** | Vulnerability hunting pipeline — inventory → audit swarm → live PoC validation → report |
| `pi-patchleak` | **private** | Patch-to-PoC diffing — compare two versions, surface the security fix, emit a leak report |
| `pi-osint` | public | OSINT / attack-surface skills — recon methodology, autopilot pipeline, cloud exposure, IdP recon |
| `pi-red` | public | 79 offensive-security skills across 23 categories — web, AD, cloud, container, wireless, exploit-dev, fuzzing, crypto, forensics |

## Updating

```sh
omp plugin marketplace update personal-toolkit   # refresh catalog
omp plugin marketplace upgrade                   # upgrade installed plugins
```

## Adding a plugin

Append an entry to `.omp-plugin/marketplace.json`. Source shape for a whole-repo plugin:

```json
{
  "name": "my-plugin",
  "description": "what it does",
  "source": { "source": "url", "url": "git@github.com:anhhung04/my-plugin.git", "ref": "main" },
  "author": { "name": "anhhung04" },
  "homepage": "https://github.com/anhhung04/my-plugin",
  "license": "MIT",
  "category": "security"
}
```

For one plugin among several in a repo use `{"source": "git-subdir", "url": "…", "path": "plugins/foo"}`.
Pin a release with `"sha": "<full-40-hex>"` instead of `"ref"`. Note `package.json#version` supplies
the installed version when the entry omits `version`.
