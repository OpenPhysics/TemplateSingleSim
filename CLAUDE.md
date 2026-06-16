# CLAUDE.md — Sim Template

Sim-specific context for AI assistants. General SceneryStack guidance: [OpenPhysics/.github/CLAUDE.md](https://github.com/OpenPhysics/.github/blob/main/CLAUDE.md).

## Project

Reusable single-screen SceneryStack template. Run `npm run rename` to fork it to a new sim name automatically. For multi-screen sims, see `doc/multi-screen.md`.

## Key files

| File | Purpose |
|---|---|
| `src/SimColors.ts` | All `ProfileColorProperty` instances |
| `src/SimConstants.ts` | Named numeric constants (layout px, physics SI units) |
| `src/SimNamespace.ts` | Namespace for color property names |
| `src/i18n/StringManager.ts` | Singleton localized string accessor |
| `src/sim-screen/SimScreen.ts` | Screen wrapper |
| `src/sim-screen/model/SimModel.ts` | Simulation state and logic |
| `src/sim-screen/view/SimScreenView.ts` | Visual nodes, layout, `screenSummaryContent` + `pdomOrder` |
| `src/sim-screen/view/SimScreenSummaryContent.ts` | Accessible screen summary (reference a11y pattern) |
| `src/sim-screen/view/SimKeyboardHelpContent.ts` | Keyboard-help dialog content |
| `src/common/SimPanel.ts` | Pre-themed `Panel` wrapper (uses `SimColors` automatically) |
| `src/common/TimeModel.ts` | Composable play/pause + elapsed-time model for animated sims |
| `scripts/generate-icons.ts` | PNG icons from `public/icons/icon.svg` |
| `scripts/rename-sim.ts` | Automated fork/rename across all files and folders |

## Common components

### SimPanel

Every control panel and info box in the sim should use `SimPanel` so that
default/projector color switching is automatic:

```typescript
import { SimPanel } from "../../common/SimPanel.js";
const panel = new SimPanel(content);              // uses SimColors defaults
const panel = new SimPanel(content, { xMargin: 20 }); // override any PanelOption
```

### TimeModel

For simulations with animation, compose `TimeModel` into your screen model:

```typescript
import { TimeModel } from "../../common/TimeModel.js";

export class FrictionModel implements TModel {
  public readonly timer = new TimeModel();   // starts paused; pass true to auto-play

  public step(dt: number): void {
    this.timer.step(dt);
    // use this.timer.timeProperty.value for physics
  }
  public reset(): void { this.timer.reset(); /* … */ }
}
```

Wire the view to `TimeControlNode` from `scenerystack/scenery-phet` binding on
`model.timer.isPlayingProperty`.

## Accessibility

This template is the **canonical accessibility reference** for OpenPhysics sims. It ships with
the three required layers wired up: PDOM names, a `SimScreenSummaryContent`, and an explicit
`pdomOrder` + `SimKeyboardHelpContent`. A11y strings live under the `a11y` key in each locale
JSON, exposed via `StringManager.getA11yStrings()`. When building a real sim, make
`currentDetailsContent` a live `DerivedProperty` over model state and add `accessibleName`s to
every interactive node. Full convention and checklist: [../Baton/ACCESSIBILITY.md](../Baton/ACCESSIBILITY.md).

## Customizing a new sim from this template

### Automated rename (recommended)

```sh
npm run rename -- --id friction --name "Friction"
# or for multi-word names:
npm run rename -- --id wave-interference --name "Wave Interference"
```

This replaces all template identifiers in file contents and renames files/folders. Run `npm run check` afterwards to verify TypeScript is clean.

### Manual checklist (if not using the rename script)

1. **Rename** — replace `sim-template` / `Sim Template` / `Sim` prefix in `init.ts`, `brand.ts`, `package.json`, class names, and screen folders
2. **Locale** — add `strings_XX.json`, register in `StringManager`, add locale to `init.ts` `availableLocales`
3. **Icon** — edit `public/icons/icon.svg`, run `npm run icons`; match theme color in `index.html` / `vite.config.ts`
4. **Colors** — edit `SimColors.ts` (`default` + `projector` profiles per property)

## Multi-screen sims

Full guide: **`doc/multi-screen.md`**

Summary:
- Create a new screen folder mirroring `src/sim-screen/` for each screen
- Add screen-name keys to all locale JSON files
- Expose new `StringProperty` getters in `StringManager.getScreenNames()`
- For shared state, create a root model passed to each per-screen model
- Register all screens in the `screens` array in `main.ts`

## Using this template beyond a direct copy

| Approach | When to use |
|---|---|
| **GitHub template** ("Use this template" button) | Starting a single new sim |
| `npm run rename` after cloning | Same, automated |
| **npm workspace / monorepo** | Managing a suite of sims with shared tooling |
| **`npm create` scaffolder** | Org-wide standardized sim bootstrapping |
| **git subtree** for pulling updates | Keeping forks in sync with template improvements |

See `doc/multi-screen.md` → "Using this template beyond a direct copy" for details on each approach.

## PWA

After `npm run build`, the sim is installable offline via Workbox (`dist/manifest.webmanifest`).
