# Lit surfaces: design decisions

A running record of what we decided for the experimental "lit surfaces" style on `/experimental`, and why.
Add to it as decisions are made, and change entries when a decision changes (say what replaced it).

The starting point is the original brief, `moth-ui-lit-surfaces.md` at the repo root. It's kept as it was;
where this file and the brief disagree, this file wins.

## The idea

Surfaces look lit from above: a soft gradient and a lit edge, so components feel slightly raised instead of flat
paint. The effect should be felt, not seen. Moth's purple glow on hover and focus stays the signature; the light
is quiet resting depth.

## How the experiment is set up

- **The official components are never restyled.** The experiment uses docs-only copies of the components in
  `ui/` that render unprefixed classes (`card`, `table-header`) instead of `m-card`, `m-table-header`. That way the
  branch can be merged to `main` and the style viewed here without changing the published packages.
- **Every rule is scoped under `.experimental`**, the wrapper from `+layout.svelte`. SvelteKit keeps a route's CSS
  loaded after you navigate away, so bare names like `.card` would otherwise style other pages.
- **The new tokens live in `experimental.css`, not `tokens.css`**, until the style is adopted. They use the final
  `--m-` names so moving them over is a cut and paste.
- **App screens:** `/experimental/dashboard` is a copy of `/examples/dashboard` using the experimental components.
  More example pages can be copied the same way.

## Tokens

Names put the CSS property first, like the existing tokens (`--m-color-surface`, `--m-shadow-focus`), so the
name tells you where it goes.

| Token | Goes on | What it does |
|---|---|---|
| `--m-gradient-surface` | `background-image` of top-level containers (cards, tables) | The light: a see-through gradient over the container's fill |
| `--m-shadow-surface` | `box-shadow` of top-level containers | The lit edge (dark) or a soft shadow underneath (light) |
| `--m-color-surface-raised-tint` | `background-color` of things inside a container (table header) | One step up from whatever it sits on |

- **Renamed for clarity.** The brief called them `--m-surface-light`, `--m-surface-sheen` and
  `--m-overlay-raised`. Those didn't say where each one goes ("sheen" and "overlay" weren't clear words for it).
- **The gradient and the shadow are separate tokens** because they go on different properties
  (`background-image` and `box-shadow`).
- **The raised tint is see-through on purpose.** A solid `--m-color-surface-raised` can't sit on a gradient: the top
  of a lit card is lighter than the solid raised color, so a table header looked sunk in instead of raised. The
  tint (gray-12 at 4% dark, 9% light) always adds one step to whatever is underneath, and nesting adds two.
  gray-12 is light in dark mode and dark in light mode, so one formula works in both.
- **Keep the solid `--m-color-surface-raised`** for anything over moving content (a sticky header, a dropdown),
  where a see-through tint would show what's underneath.

## Where the light goes

- **Only top-level containers get light** (cards and table wrappers so far). Anything inside them gets the raised
  tint, never its own light: one light source per container.
- **Card `flat`** turns the light off, for a card inside a card.
- **Tall surfaces only light their top (dark mode).** The gradient ends at 160px instead of stretching over the
  whole height, which made tall cards muddy at the bottom.
- **Data stays flat.** Chart bars and progress bars never get a gradient. (The brief also kept information-only
  badges flat; badges are being tried lit, see Badge.)

## Dark mode

- Gradient: 4% white at the top, gone by 160px.
- Shadow: a 1px inset highlight along the top edge, 7% white.

## Light mode

- **Cards aren't flat in light mode** (the brief said they should be). White can't get lighter, so the light shows
  the other way round: the top stays white and the card settles slightly darker towards the bottom (clear down to
  40% of the height, then up to 2.5% gray-12 at the bottom edge).
- **Depth comes from a shadow instead of a highlight.** `--m-shadow-surface` becomes a faint inner bottom edge
  (5%), a tight 1px drop shadow (6%) and a wider soft one (6px down, 16px blur, pulled in by 6px so it stays under
  the card, 10%), all gray-12.
- **Open question:** on tall tables the bottom tint fades in over the lower 60% of the height. If that reads as
  muddy, end it a fixed distance from the bottom, like the dark-mode gradient's 160px.

## Button

- **Primary and danger get the light; secondary, tertiary and disabled stay flat.** A gray filled pill with a
  gradient is exactly the YouTube button, so secondary stays outlined. Every flat variant turns off both the
  gradient and the highlight, otherwise it keeps the purple gradient from the base rule.
- **The strongest effect in the system**, because strength follows importance: the gradient runs the full height
  (about 15% white at the top, the plain fill at 25%, about 12% black at the bottom) with a 22% white highlight on
  the top edge. Containers get much less.
- **The lighter part stays above the label.** The brief put the plain fill at 55%, which left extra white behind the
  top half of the label: the danger label measured about 4.2:1 even at rest. The fill now arrives at 25% of the
  height, where the label's line box starts, so the light reads along the top and the label keeps 4.5:1.
- **One color per variant.** The gradient is built from `--m-button-fill`; a variant changes only that (danger
  sets `--m-color-danger`), never the gradient itself. Future variants are one line each.
- **Component variables say where they go**, like the tokens: `--m-button-gradient` (background-image) and
  `--m-button-shadow` (box-shadow at rest). The brief called them `tone`, `surface` and `sheen`.
- **No ring around the edge.** The border is transparent and the gradient runs under it
  (`background-origin: border-box`). A solid border in the fill color showed as a lighter ring around the darker
  bottom of the gradient.
- **The highlight is drawn by `::after`, not the button's own box-shadow.** An inset box-shadow is drawn inside
  the border, so with the 1px transparent border the highlight sat 1px below the top edge, with a strip of
  gradient above it. `::after` covers the border box (`inset: -1px`) and carries `--m-button-shadow`, so the
  highlight is on the very top row. It brightens when the button is lit.
- **Built from the current official CSS, not the brief's snapshot**, so the secondary disabled style and the
  secondary focus border are kept.
- **Hover is more light shining on the button, not a glow (primary and danger).** The official lamp hover
  (white-to-lavender light, dark label, wide halo) felt like the button itself was glowing. Now the light from
  above gets brighter, and it fades in all at once (0.2s), like the original:
  - `--m-button-lit-background` (in `::before`): a radial light from the top center, 45% white at the top, 12% at
    half its reach, gone by 25% of the height, before the label;
  - the top-edge highlight brightens from 22% to 60% white (`--m-button-lit-shadow`, on `::after`);
  - the label stays white (`--m-button-lit-color` is the on-accent / on-danger text color);
  - the halo is small and close (`0 2px 12px -4px`, 60% of the fill) instead of the wide 24px glow;
  - the grow (`scale(1.03)`) and the focus ring are unchanged.
  `isolation: isolate` lets the `::before` layer's `z-index: -1` paint above the button's background.
- **The light stops before the label, so the label can stay white.** A light covering the whole button (up to
  50% white at the top, 18% at the bottom) put the white label at about 3.7:1. Two fixes were tried: turning the
  label dark when lit (passed easily, but dropped in favor of keeping the white label), and the current one,
  keeping the light in the top band. Measured behind the label's whole line box, down the center where the light
  is strongest: primary at least 5.4:1 and danger 4.68:1 (the danger fill itself), at rest and lit, both sizes,
  both themes.
- **Tried and dropped: a sweeping shine.** A diagonal streak of light that crossed the button once on hover. It
  didn't feel right: the change should happen all at once, like the original hover.
- **Secondary and tertiary keep the official hover** (outline turns the text color and glows; gray pill). They
  have no fill to shine light on.
- **Same in both themes.** Accent and danger fills are dark enough in light mode that the white highlight still
  reads.
- **Contrast rule:** anything lighter than the fill (the resting gradient's top, the hover light) has to end by 25%
  of the button's height, where the label starts. Check it again whenever either changes.

## Badge

- **Tried lit, against the brief.** The brief kept information-only badges flat ("data stays flat"). They're lit
  here to see how it looks; if they read as too much, going back is removing the Badge rules' gradients.
- **Soft status badges are small banners:** the same gradient (top blends 15% towards
  `--m-color-<status>-weak-highlight`, the fill by 60%, `--m-weak-shade` black at the bottom in dark mode only)
  but no top highlight. They keep their 2px border (soft status components have one), and a highlight just inside
  it read as a second, lighter outline (tried and dropped). The border already marks the edge.
- **Strong status badges are small primary buttons:** 15% white at the top, the plain fill by 20% of the height
  (the label starts at 22%), 12% darker at the bottom, and a 22% white highlight. The border is dropped (it was the
  fill color anyway) and the padding grows by its 2px, so sizes match the official badge exactly and the
  highlight is on the very top edge.
- **Strong warning darkens towards its own deep amber (`--m-warning-11`), not black.** Black turned the bright
  yellow mustard. In dark mode step 11 is itself a light yellow, so the bottom barely darkens there.
- **Neutral badges stay flat, soft and strong.** A gray pill with a gradient is the YouTube button.
- **The banner's highlight variables are shared:** renamed from `--m-banner-highlight-<status>` /
  `--m-banner-shade` to `--m-color-<status>-weak-highlight` / `--m-weak-shade`, since they belong to the soft
  status fills, not to one component.
- **Contrast**, behind the label's line box: soft badges at least 9:1; strong info, success and danger 4.68 to
  4.70:1 (their fill, the light ends before the label), strong neutral 5.1:1, strong warning 11.8:1 and up.

## Input

- **Fields are set into the surface, the opposite of buttons.** Under one light from above, a raised thing (button,
  card) catches light along its top; a recessed thing shows the shadow its top lip casts inside it. A field is a slot
  you type into, so it's recessed: `--m-shadow-field`, a soft inner shadow along the top (dark mode also a faint
  6% white catch-light along the bottom lip). That makes buttons and fields read as opposites at a glance, from the
  same light.
- **Values.** Dark: `inset 0 2px 4px` 70% black plus `inset 0 -1px 0` 6% white. Light: `inset 0 1px 2px` 10%
  gray-12 (a white catch-light doesn't show on a white field). The dark shadow started at `1px 2px` 45% and was
  invisible on the near-black fill.
- **Focus lights the field up instead:** the shadow goes, and the field gets the official focus (raised fill,
  `--m-color-border-control-focus`, `--m-shadow-focus`). Disabled is flat (no shadow). Everything else (shape,
  border, padding, hover, error) is the official field: fields stay subdued, and they don't get their own light,
  since they sit inside lit cards.
- **Tried and dropped: a pill with a lit rim**, modeled on a reference chat input: a gradient border, brighter on
  top, the edge painted under a transparent border; `--m-radius-full`; labels indented to the pill's text. It looked
  bad. Lesson: the light should come from the system's own model (raised vs recessed), not from copying a reference.
- **Not done yet:** Select and Textarea share the field look and should follow.

## Banner

- **Lit like a card, but the light takes the banner's own color.** The top blends 15% towards a highlight color,
  the plain fill arrives at 60% of the height, and a 1px highlight (45% of the highlight color) runs along the top
  edge. Banners are short, so the gradient runs the full height.
- **Dark mode blends towards a brighter step (7) of the same scale, not white.** White mixed into a dark,
  saturated color greys it out: the warning banner's top turned olive. With step 7 it reads golden.
- **Light mode blends towards white,** which brightens pastels cleanly.
- **Light mode doesn't darken the bottom.** Dark mode mixes 6% black into the bottom; on light pastels that greyed
  them out (muddy yellow and pink, the same problem as white on dark colors), so light mode settles at the plain
  fill (`--m-weak-shade: 0%`).
- **Theme values live with the tokens, not in `[data-theme="light"] .banner` rules** (CLAUDE.md rules those out, so
  nested themes work): `--m-color-<status>-weak-highlight` and `--m-weak-shade`. The brief used a light-theme
  selector here.
- **One pair of values per variant.** The gradient and highlight are built from `--m-banner-fill` and
  `--m-banner-highlight`; a variant changes only those. Renamed from the brief's `tone` and `glint`.
- **No border, so the highlight is a plain inset box-shadow** (it already sits on the very top edge; the button
  needed `::after` only because of its border).
- **Contrast:** the text keeps at least 9.3:1 behind it on every variant in both themes, measured across the
  text's line box.
- **Light-mode highlight (brief open item):** 45% white on a pastel top barely shows, and it isn't too strong. Kept.

## More contrast

Cards and table wrappers get a `--m-color-border` edge, like the official ones, with `background-origin:
border-box` so the gradient runs under the border (otherwise a ring shows around the edge).

## Not done yet

Modals/popovers/toasts, and the other open items in the brief (section 6).
