# Brand reference — Integrated Projects (IPX)

**This is a hard default.** Every `/brag` video uses this palette and typography, regardless of what
the source project looks like. A project with its own CSS does not override it. Step 1 still reads
the project's colours and fonts, but only to *describe* the product — never to restyle the video.

The one thing that does come from the project is **product-native colour**: a palette the product
itself produces and that a viewer would recognise as part of its output (a chart series, a rendered
artefact, a canvas). Those appear inside the product imagery and keep their exact values. Everything
around them — background, type, accents, rules — is IPX.

Company: **Integrated Projects**, or **IPX**. Never "IP" alone externally.
Products: **SCANIT** and **BIMIT**, always capitalised.

---

## Source of truth

Values below are taken from the IPX UI kit:

```
github.com/Integrated-Projects/ip-ui-platform
  packages/ui/src/globals.css      <- authoritative runtime tokens (HSL)
  packages/ui/src/theme/colors.ts  <- named brand constants
  skills/ip-typography.md          <- type scale
```

**The shipped UI package wins.** `packages/ui/src/globals.css` is what actually renders in product,
and several other files in that repo have drifted from it. When they disagree, use globals.css:

| Token | `packages/ui/src/globals.css` | Stale copies elsewhere in the kit |
|---|---|---|
| dark background | `0 0% 10%` = **`#1A1A1A`** | `#141414` in `theme/colors.ts`, `packages/cli/README.md`, `ui-mcp-server/index.js` |
| warning | `38 92% 35%` = **`#AB6F07`** | `#F59E0B` in `styles.css` and `apps/docs/globals.css` |
| dark muted text | `0 0% 64%` = **`#A3A3A3`** | `#A8A8A8` in `theme/colors.ts` |

Where the CSS HSL rounds slightly off a named brand constant, **use the named hex**: `--primary`
computes to `#3881FF` but the brand blue declared in three places is `#3779FF`. Same for
`--primary-dark` (`#2250A0` computed, `#214CA3` named).

---

## Palette

| Role | Hex | Use in video |
|---|---|---|
| Dark background | `#1A1A1A` | Dark-scene background. Anything measured, raw, unresolved. |
| Offwhite | `#F0F0F0` | Light-scene background. Anything drafted, finished, delivered. Never pure white for a page. |
| Card / plate | `#FFFFFF` | Elevated surfaces only — a sheet holding product imagery. |
| Elevated dark | `#262626` | A raised panel on a dark scene. |
| Text Gray | `#2A2A2A` | Primary text on light. |
| Secondary Light | `#5E5E5E` | Captions, labels, supporting figures on light. |
| Offwhite text | `#F0F0F0` | Primary text on dark. |
| Secondary Dark | `#A3A3A3` | Captions and labels on dark. |
| IP Blue | `#3779FF` | Accent. See the contrast rule before using it on text. |
| IP Light Blue | `#5794FF` | Blue text on a dark background. |
| IP Dark Blue | `#214CA3` | Blue text on a light background. |
| Border Gray | `#D9D9D9` | Hairlines, panel edges on light. |
| Dark Border | `#333333` | Hairlines on dark. |

Status colours — `#22C55E` success, `#AB6F07` warning, `#EF4444` error — carry meaning and are never
decorative. Use them only when the video genuinely shows success, warning, or failure state.

### Contrast rule (measured against the kit's real backgrounds)

`hyperframes check` gates contrast as an **error**, so this is a build failure, not a style note.

| Pair | Ratio | AA normal (4.5) | AA large (3.0) |
|---|---|---|---|
| `#3779FF` on `#1A1A1A` | 4.43 | **fail** | pass |
| `#5794FF` on `#1A1A1A` | 5.87 | pass | pass |
| `#214CA3` on `#1A1A1A` | 2.18 | **fail** | **fail** |
| `#F0F0F0` on `#1A1A1A` | 15.27 | pass | pass |
| `#A3A3A3` on `#1A1A1A` | 6.90 | pass | pass |
| `#F0F0F0` on `#262626` | 13.28 | pass | pass |
| `#3779FF` on `#F0F0F0` | 3.45 | **fail** | pass |
| `#214CA3` on `#F0F0F0` | 7.01 | pass | pass |
| `#2A2A2A` on `#F0F0F0` | 12.60 | pass | pass |
| `#5E5E5E` on `#F0F0F0` | 5.69 | pass | pass |
| `#3779FF` on `#FFFFFF` | 3.93 | **fail** | pass |
| `#214CA3` on `#FFFFFF` | 7.99 | pass | pass |

**The rule.** IP Blue `#3779FF` fails AA for normal-size text on *every* IPX background, including
the dark one. It is safe as a large display figure, a rule, or a shape — never as a caption or a
sentence.

- Blue text on **dark** → IP Light Blue `#5794FF`
- Blue text on **light** → IP Dark Blue `#214CA3`
- Never IP Dark Blue on dark. It measures 2.18 and fails both thresholds.

---

## Typography

System stack, no web fonts. On this machine that resolves to SF Pro, which is the intent.

| Role | Weight | Tracking | Notes |
|---|---|---|---|
| Display | 300 | -0.025em | Hero lines, scene headlines, big figures |
| Heading | 500 | -0.025em | Rare in video; prefer display |
| Body | 400 | normal | Supporting sentences |
| Mono label | 400 | 0.01em | Filenames, dimensions, metrics, technical captions |
| Overline | 500 | 0.05em | UPPERCASE small label |

The kit's web scale is 60/40px display, 40/32/24/20/18/16px headings, 16/15/13px body, 12px minimum.
**Video scales up from that** — a 1920x1080 frame reads at roughly 3x a web layout. Keep the kit's
*ratios* and weights, not its pixel sizes. A scene headline near 56-62px, a hero figure near
110-170px, and a mono label near 21-30px all sit correctly against the kit's proportions.

**Declare the families with generic keywords only:**

```css
--sans: ui-sans-serif, system-ui, sans-serif;
--mono: ui-monospace, monospace;
```

Do **not** name families like `"Segoe UI"`, `"Helvetica Neue"`, `Roboto`, `Menlo`, or `SFMono-Regular`
in a composition, even though the kit's own web stack lists them. `hyperframes lint` fires
`font_family_without_font_face` on a named family with no in-file `@font-face` pointing at a shipped
local file, and the render is blocked. The generic keywords resolve to the same system fonts and pass
lint.

Figures and stat rows use `font-variant-numeric: tabular-nums` so numbers do not jitter while
animating.

---

## How the brand behaves in a video

**Gray-first.** The frame is dark or offwhite. Colour appears with purpose or not at all.

**One accent per scene.** IP Blue marks the single thing that matters in that scene — one figure, one
rule. A scene with two blue elements has no accent, it has decoration.

**Darkness means unresolved.** Use `#1A1A1A` for the problem, the raw input, the measurement. Use
`#F0F0F0` for the answer, the artefact, the result. When a video has a turning point, let the
background lift from dark to offwhite *on* that beat rather than at a scene boundary. The lift is an
argument, not a transition.

**No decoration.** No gradients as style, no ornamental borders, no background patterns, no particle
fields, no glows that are not carrying audio-reactive data. Whitespace is the design. A radial
ambience tied to the music bed is allowed because it is doing a job; a gradient because it looks nice
is not.

**Hairlines, not boxes.** A 2px IP Blue rule of about 130px is the house divider. Prefer it to a
card, a panel, or a border whenever it will do.

**Product imagery sits on white.** When real product output appears (a render, a plan, a chart, a
screenshot), put it on a `#FFFFFF` plate against the offwhite frame. It reads as a sheet of paper and
separates the artefact from the page without a border.

**Information density is fine.** These are professional AEC tools. A frame carrying three real
figures and a filename is on-brand; a frame carrying one word in 200px type is not, unless the tone
is `deadpan` or `cinematic`.

---

## Tone interaction

The brand fixes palette and type. The tone preset in `tones.md` still sets case, scale, pacing, and
transition character. Where a tone's typography note conflicts with this file, **this file wins on
colour, family, weight, and tracking**; the tone wins on case, relative size, and rhythm.

Two tones need an explicit ruling:

- `chaotic` asks for ALL CAPS, oversized, tilted type. Keep the caps and the scale. Drop the tilt and
  keep the IPX weights — tilted type is decoration.
- `cinematic` asks for ALL CAPS at significant scale. Allowed, at weight 300, tracking -0.025em.

---

## Video-safe defaults

Reach for these unless a scene argues otherwise.

```
Dark scene   bg #1A1A1A  text #F0F0F0  caption #A3A3A3  accent #3779FF (display) / #5794FF (text)
Light scene  bg #F0F0F0  text #2A2A2A  caption #5E5E5E  accent #3779FF (display) / #214CA3 (text)
Plate        bg #FFFFFF  soft drop shadow, no border
Rule         2px x 130px #3779FF
Display      weight 300, tracking -0.025em, tabular-nums
Mono label   21-30px, tracking 0.01em
```
