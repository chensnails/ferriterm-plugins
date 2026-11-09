# Ferriterm plugins

Marketplace index for [Ferriterm](https://github.com/chensnails/ferriterm). The
desktop app reads `index.json` from the `main` branch in **Settings › Plugins ›
Marketplace**, then downloads the manifest of whichever plugin you install.

```
index.json                     marketplace index (entry point)
plugins/<id>/manifest.json     plugin manifests referenced by the index
```

## Adding a plugin

1. Create `plugins/<id>/manifest.json`. The file name and `id` must match and
   may only contain ASCII letters, digits, `.`, `_` and `-`.
2. Add an entry to `index.json` whose `manifestUrl` points at that file over
   **https**.
3. Open a pull request.

Supported kinds and their required fields:

| `kind` | Required fields |
| --- | --- |
| `language` | `locale`, `strings` (object) |
| `theme` | `base` (`dark` or `light`), `tokens` (object of `--css-variable` → value) |
| `command-pack` | `groups` (`[{ "name": "...", "commands": [{ "name": "...", "template": "..." }] }]`) |
| `wasm` | `entry` (relative `.wasm` path inside the plugin directory) |
| `serial` | `permissions: ["serial"]`; unlocks the built-in serial transport |

Every manifest also needs `id`, `name`, `kind` and `apiVersion: 1`. Optional
fields are `version`, `description` and `permissions` (`network`, `filesystem`,
`clipboard`, `serial`).

The app validates the whole index before showing it: ids must be unique, kinds
must be known, and every URL must be public https. Installing writes a single
`<id>.json` manifest into the app's plugin folder, so uninstalling is a single
file removal.

## Notes

- The serial transport is implemented in Ferriterm itself. The `serial` plugin
  is a permission switch, not native code shipped from this repository.
- Keep `index.json` free of tracking links; the app shows `author`,
  `description` and `homepage` as-is.
