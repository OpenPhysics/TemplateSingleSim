# CLAUDE.md — Sim Template

Sim-specific context for AI assistants. General SceneryStack guidance: [OpenPhysics/.github/CLAUDE.md](https://github.com/OpenPhysics/.github/blob/main/CLAUDE.md).

## Project

Reusable single-screen SceneryStack template. When forking, search-and-replace `sim-template` / `SimTemplate` / `Sim Template` / `SimModel` / `SimScreen` throughout.

## Key files

| File | Purpose |
|---|---|
| `src/SimColors.ts` | All `ProfileColorProperty` instances |
| `src/SimNamespace.ts` | Namespace for color property names |
| `src/i18n/StringManager.ts` | Singleton localized string accessor |
| `src/sim-screen/SimScreen.ts` | Screen wrapper |
| `src/sim-screen/model/SimModel.ts` | Simulation state and logic |
| `src/sim-screen/view/SimScreenView.ts` | Visual nodes and layout |
| `scripts/generate-icons.ts` | PNG icons from `public/icons/icon.svg` |

## Customizing a new sim from this template

1. **Rename** — replace template identifiers in `init.ts`, `brand.ts`, `package.json`, and screen folders
2. **Locale** — add `strings_XX.json`, register in `StringManager`, add locale to `init.ts` `availableLocales`
3. **Icon** — edit `public/icons/icon.svg`, run `npm run icons`; match theme color in `index.html` / `vite.config.ts`
4. **Colors** — edit `SimColors.ts` (`default` + `projector` profiles per property)

## PWA

After `npm run build`, the sim is installable offline via Workbox (`dist/manifest.webmanifest`).
