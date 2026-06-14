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
| `src/sim-screen/view/SimScreenView.ts` | Visual nodes, layout, `screenSummaryContent` + `pdomOrder` |
| `src/sim-screen/view/SimScreenSummaryContent.ts` | Accessible screen summary (reference a11y pattern) |
| `src/sim-screen/view/SimKeyboardHelpContent.ts` | Keyboard-help dialog content |
| `scripts/generate-icons.ts` | PNG icons from `public/icons/icon.svg` |

## Accessibility

This template is the **canonical accessibility reference** for OpenPhysics sims. It ships with
the three required layers wired up: PDOM names, a `SimScreenSummaryContent`, and an explicit
`pdomOrder` + `SimKeyboardHelpContent`. A11y strings live under the `a11y` key in each locale
JSON, exposed via `StringManager.getA11yStrings()`. When building a real sim, make
`currentDetailsContent` a live `DerivedProperty` over model state and add `accessibleName`s to
every interactive node. Full convention and checklist: [../ACCESSIBILITY.md](../ACCESSIBILITY.md).

## Customizing a new sim from this template

1. **Rename** — replace template identifiers in `init.ts`, `brand.ts`, `package.json`, and screen folders
2. **Locale** — add `strings_XX.json`, register in `StringManager`, add locale to `init.ts` `availableLocales`
3. **Icon** — edit `public/icons/icon.svg`, run `npm run icons`; match theme color in `index.html` / `vite.config.ts`
4. **Colors** — edit `SimColors.ts` (`default` + `projector` profiles per property)

## PWA

After `npm run build`, the sim is installable offline via Workbox (`dist/manifest.webmanifest`).
