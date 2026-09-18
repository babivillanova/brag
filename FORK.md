# IPX fork of `brag`

Fork of [latent-spaces/brag](https://github.com/latent-spaces/brag). Everything upstream does still
works. The one behavioural change is that **branding is fixed** rather than inferred from the source
project.

## What diverges from upstream

| File | Change |
|---|---|
| `skills/brag/references/brand.md` | **New.** The IPX house palette and typography, taken from the IPX UI kit, plus a measured WCAG contrast table. |
| `skills/brag/SKILL.md` | Added a "Branding is fixed" section; Step 2 now reads `brand.md`. |
| `skills/brag/references/step-1-inspect.md` | Colour and font extraction now describe the *product*, not style the video. Rubric Q3 no longer asks for a palette. |
| `skills/brag/references/step-2-plan.md` | Plan template's visual-identity block carries the fixed IPX values. |
| `skills/brag/references/step-3-compose.md` | Composition brief's visual-identity block carries the fixed IPX values and the contrast rule. |
| `skills/brag/references/tones.md` | Header note: tone controls case, scale, and density; it no longer controls colour, family, weight, or tracking. |
| `.claude-plugin/*.json` | Version suffixed `-ipx.N`, description and repo point at this fork. |

**Unchanged:** music (`assets/music/`), SFX, `references/audio.md`, `references/step-4-deliver.md`,
the scripts, and every creative law. Upstream owns those.

## Where the brand values come from

```
github.com/Integrated-Projects/ip-ui-platform
  packages/ui/src/globals.css      <- authoritative runtime tokens
  packages/ui/src/theme/colors.ts  <- named brand constants
  skills/ip-typography.md          <- type scale
```

`packages/ui/src/globals.css` is the source of truth. Several other files in that repo have drifted
from it — `brand.md` records which and why. If the UI kit changes, re-check that file and update
`brand.md`; nothing else in this fork holds colour values.

## Pulling upstream changes

```bash
git fetch upstream
git merge upstream/main
```

Conflicts should be small and predictable: they land in `SKILL.md` and the four `references/step-*.md`
files, at the blocks listed in the table above. `brand.md` is ours alone and will never conflict.

After merging, bump the `-ipx.N` suffix in `.claude-plugin/plugin.json` so Claude Code picks up the
new version, then reinstall:

```bash
/plugin marketplace update brag-ipx
```

## Installing

```bash
/plugin marketplace add babivillanova/brag
/plugin install brag@brag-ipx
```

Invoke with `/brag`. Flags are unchanged from upstream (`--tone`, `--format`, `--duration`,
`--no-music`, `--no-sfx`, `--title`, `--voice`).
