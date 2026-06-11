# mim-registry

The package registry for [Mim](https://github.com/bitowaqr/shoulders-v0-3). One file, no server: the Mim app clones this repo, reads `index.json`, and shows the listed packages in its Apps surface.

## How installs work

Each entry pins a package to an exact git commit. The installer clones `repo`, checks out `ref`, verifies HEAD equals `commit` (refusing moved tags), and copies only `path` (when set) into `~/.mim/packages/<id>/<version>/`. Declared `permissions` must match the package manifest byte-for-byte or the install is refused.

Most entries point at the [shoulders-ai/mim-packages](https://github.com/shoulders-ai/mim-packages) monorepo via `path`, but any public HTTPS git repo works.

## Entry schema

```json
{
  "id": "slides",
  "name": "Slides",
  "description": "One line shown in the registry browser.",
  "repo": "https://github.com/shoulders-ai/mim-packages",
  "path": "packages/slides",
  "version": "0.1.0",
  "ref": "slides-v0.1.0",
  "commit": "<full 40-char SHA>",
  "permissions": { "workspace": { "read": true, "write": true } },
  "engines": { "mim": "runtime-v1" }
}
```

- `repo` must be HTTPS. `commit` must be the full 40-character SHA the `ref` resolves to.
- `path` (optional) is a repo-relative subdirectory with no `.` or `..` segments; omit it when the package is the repo root.
- `version` must match the package manifest's version exactly.
- `permissions` must equal the package manifest's `mim.permissions` exactly.

## Adding or updating a package

1. Tag the release in the package repo (`<id>-v<version>` in mim-packages).
2. Open a PR here editing `index.json` with the new `version`, `ref`, and `commit`.
3. Review = reading the pinned diff in the package repo. Merging publishes.
