# Mim Package Registry

Central registry for [Mim](https://github.com/shoulders-ai) packages. Everything goes through the GitHub org **shoulders-ai**.

## How it works

This repo contains a single `index.json` file that lists available packages with their repo URLs, pinned commits, declared permissions, and engine requirements. The Mim runtime clones this repo as a local mirror and reads the index — no server needed.

### For users

```
registry.list                              # browse available packages
package.install { id: "github-monitor" }   # install from registry
package.update  { id: "github-monitor" }   # update to latest
```

Or in the Mim UI: **Package Manager → Browse registry → Install**.

### For package authors

To add a package, open a PR that adds an entry to `index.json`:

```json
{
  "id": "your-package",
  "name": "Human Name",
  "description": "One line.",
  "repo": "https://github.com/shoulders-ai/mim-your-package",
  "version": "1.0.0",
  "ref": "v1.0.0",
  "commit": "<full 40-char git sha>",
  "permissions": { "http": ["api.example.com"], "secrets": ["api_key"] },
  "engines": { "mim": "runtime-v1" }
}
```

**Requirements:**
- `repo` must be HTTPS
- `commit` must be the full SHA that `ref` points to (tag moves are rejected at install time)
- `version` must be valid semver (`x.y.z` with optional prerelease)
- `permissions` must exactly match the package's `package.json` `mim.permissions` — mismatches are refused at install time
- No submodules, no symlinks in the package tree

Curation = PR review. The registry is append-only by convention; removal requires a separate PR with justification.
