# cg-extensions-registry

The central catalogue of installable [Consigliere](https://github.com/mnemcik/consigliere)
extensions. As of `cg` v1.6.0, installs are fully qualified: `cg extension install cg/<name>`
resolves `<name>` in this registry (the built-in `cg` alias points here), then clones the
extension's own repo. Bare names are rejected.

This registry is deliberately a **separate repo** from `cg` so the catalogue can
grow without a `cg` binary release.

## What's here

| File | Purpose |
|------|---------|
| [`index.json`](index.json) | The catalogue — a versioned array of extension entries. |
| [`schema/index.schema.json`](schema/index.schema.json) | JSON Schema for `index.json`. |
| [`schema/cg-extension.schema.json`](schema/cg-extension.schema.json) | JSON Schema for an extension's `cg-extension.json` manifest (v1, and v2 from cg 2.0). |

## Adding your extension

1. Publish your extension repo with a `cg-extension.json` at its root (see the
   [authoring guide](https://github.com/mnemcik/consigliere/blob/main/EXTENSIONS.md)).
2. Tag a release.
3. Open a PR adding an entry to `index.json`:

   ```json
   {
     "name": "your-ext",
     "description": "One-line summary",
     "repo": "https://github.com/you/cg-ext-your-ext",
     "latestVersion": "0.1.0",
     "manifestUrl": "https://raw.githubusercontent.com/you/cg-ext-your-ext/main/cg-extension.json"
   }
   ```

Entries are validated against `schema/index.schema.json`. `name` must be unique
and match `^[a-z0-9-]+$`.

## How a PR gets to merge

This registry follows the `cg-extensions` flow: see
[How a PR gets to merge](https://github.com/mnemcik/cg-extensions#how-a-pr-gets-to-merge)
in its README for the roles (author, maintainer, reviewers) and the impact review.
Every change starts as an issue, adding an extension included: open the issue first,
and the PR in step 3 above says `Fixes #NNN`.

Here the required check is **`impact-review`**, a commit status on the PR's head
commit. The maintainer sets it on this repository after the impact review; until it
is `success`, the PR can't merge. A status belongs to one commit, so a push,
including an update from `main`, needs a new review.

Differences from `cg-extensions`: no approving review is required, there is no PR
template, and there is no Impact Review workflow, so the maintainer sets the status
on every PR, bot PRs included.

## Trust model

The registry is curated by PR. Consigliere v1 does **not** sign or verify
extensions — installing one runs its contributions against your workspace.
Install only extensions you trust.
