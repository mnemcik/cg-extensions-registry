# cg-extensions-registry

The central catalogue of installable [Consigliere](https://github.com/mnemcik/consigliere)
extensions. `cg extension install <name>` resolves `<name>` here, then clones the
extension's own repo.

This registry is deliberately a **separate repo** from `cg` so the catalogue can
grow without a `cg` binary release.

## What's here

| File | Purpose |
|------|---------|
| [`index.json`](index.json) | The catalogue — a versioned array of extension entries. |
| [`schema/index.schema.json`](schema/index.schema.json) | JSON Schema for `index.json`. |
| [`schema/cg-extension.schema.json`](schema/cg-extension.schema.json) | JSON Schema for an extension's `cg-extension.json` manifest (v1). |

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

## Trust model

The registry is curated by PR. Consigliere v1 does **not** sign or verify
extensions — installing one runs its contributions against your workspace.
Install only extensions you trust.
