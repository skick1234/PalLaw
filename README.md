# PalLaw Rules Studio

[PalLaw Rules Studio](https://pallaw.palorbit.app/) is a free browser editor for PalLaw server rules. Draw regions on the map, configure rules, and validate and export a `PalLaw.json` file for your server. Imported configurations and saved drafts stay in your browser.

## Use the editor

Open [Rules Studio](https://pallaw.palorbit.app/), import an existing `PalLaw.json` or start with an example, then edit your regions and rules. Export the configuration and copy it to your server's PalLaw mod directory.

Get the mod on [Nexus Mods](https://www.nexusmods.com/palworld/mods/4193). For help, join [Discord](https://discord.gg/zzhK54aaYz); report editor bugs on [GitHub](https://github.com/skick1234/PalLaw/issues).

## Unofficial project and third-party assets

I created PalLaw Rules Studio as an unofficial project. It is not affiliated with, endorsed by, sponsored by, or approved by Pocketpair, Inc. Palworld and all related names, trademarks, map imagery, and game assets are the property of their respective owners. I do not claim ownership of those materials.

The Apache License 2.0 applies only to this project's original source code and documentation. It does not license or grant rights to Palworld trademarks, map imagery, or other third-party assets. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for provenance and license details.

Pocketpair's current fan-content terms are available in its [Guidelines for Derivative Works](https://www.pocketpair.jp/en/guidelines-derivativework-en/). Rights holders may report concerns through this repository's issue tracker.

## Static-site development

Legal notices and PalLaw Rules Studio are one multi-page SolidJS application built by a single TypeScript/Vite project. Run all static-site commands from this repository with Bun:

```powershell
bun install --frozen-lockfile
bun run typecheck
bun run test
bun run build
```

Page source lives under `apps/legal/` and `apps/pallaw/`; shared Solid controls live under `apps/shared/`. `site/index.html` opens Studio and `site/legal/index.html` holds legal notices. Map, vendor, and configuration assets remain under `site/pallaw/`. PalLaw's deeper application source lives under `apps/pallaw/src/`:

- `domain/` parses, migrates, hydrates, validates, evaluates, and serializes the public configuration contract without DOM, storage, Solid, or Leaflet dependencies.
- `document/` owns immutable snapshots, bounded undo/redo history, dirty state, import/export, validation, and draft persistence. Accepted commands publish and persist at most once.
- `editor/` adapts document snapshots to Solid view state and reconciles selection after document changes.
- `map/` is the only module that accesses `window.L`. Its `MapController` interface accepts PalLaw coordinates and owns Leaflet listeners, layers, drawing, editing, moving, fitting, resize observation, and disposal.
- `ui/` contains the single Solid application root and feature components. Components emit intent-level actions rather than mutating document snapshots.

The Vite build writes page entries, shared chunks, and PalLaw CSS to the ignored `site/build/` directory. GitHub Pages publishes the built `site/` directory.

Use the Bun version pinned in `package.json`. The Pages workflow installs dependencies, type-checks, tests, and builds before publishing. The site's content security policy allows local scripts and blocks outgoing data requests. The Ko-fi donation iframe loads only when a visitor opens Donate.

The Pages workflow uses `tools/stamp-site-cache.mjs` to update CSS and JavaScript cache keys during publication.

Automated checks do not replace native browser, responsive, download, shared-script, or interactive-map verification. Complete [`docs/PALLAW_MANUAL_CHECKLIST.md`](docs/PALLAW_MANUAL_CHECKLIST.md) before publishing a frontend change.

## Transfer a saved draft

If you used Studio at its previous address, export your saved draft there as `PalLaw.json`, then import it at the new address. Browser storage does not transfer between addresses.
