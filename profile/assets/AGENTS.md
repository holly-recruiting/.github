# Holly recruiting assets

Public-facing visual assets for Holly's GitHub recruiting profile. Keep every asset crisp, readable
at GitHub's rendered width, and consistent with the existing SVG system.

## Scope

- `engineering-benefits.svg`, `company-benefits.svg`, `engineering-ownership.svg`, and
  `interview-process.svg` are hand-composed, self-contained SVG illustrations.
- `tech-stack/` contains exported product and tooling logos. Preserve their original SVG geometry.
- `join-holly.svg` is the footer call-to-action.
- `banner.png` is the only intentional raster asset in this directory.

## Visual system

Use the established values unless the surrounding asset requires an existing variation:

| Role | Value |
| --- | --- |
| Default benefit canvas | `960 × 540` |
| Background | `#080c14` |
| Card | `#101623` |
| Featured card | `#151d2a` |
| Border | `#2c3a4d` |
| Accent | `#818cf8` |
| Accent light | `#a5b4fc` |
| Accent text | `#c7d2fe` |
| Primary text | `#ffffff` |
| Muted text | `#94a3b8` |
| Heart | `#f87171` |
| Font stack | Geist, system UI fallbacks |

- Use 10–12 px corner radii and the existing translucent accent circles behind utility icons.
- Align related cards to shared horizontal and vertical guides. Similar cards use identical padding.
- Keep the hierarchy obvious at a glance: section label, card title, value, then supporting text.
- When benefits are peers, give every card identical dimensions and visual weight. Do not make one
  category a hero card unless the content explicitly calls for that hierarchy.
- Benefit cards use one of two copy structures: title → headline → description, or title → headline
  → description → detail. Never show a detail without a description.
- Keep card titles short and evocative; put the literal benefit in the headline or description. When
  iterating on body copy, keep the icon-and-title row fixed and move the entire content block together.
- Keep details visually close to the description they qualify. Use color and weight—not a large gap—to
  separate the two levels.
- Reorder benefit cards by recruiting impact and scan priority. Put the strongest differentiators in
  the first row, then supporting day-to-day benefits below.
- Do not shrink text to rescue an overcrowded layout. Increase the card or canvas instead.
- Shorten copy or wrap a headline onto two intentional lines before reducing type. Use a dedicated
  feature-headline size when prose replaces a numeric value.
- Preserve generous outer margins and consistent gaps between cards.
- Treat badges as a single lockup. Center the combined text and icon bounds inside the pill rather than
  centering each child independently.

## Icons

For icons newly embedded in hand-composed illustrations, use one of these sources. Do not mix in a
third icon family without explicit approval. Exported official logos in `tech-stack/` are exempt;
preserve the vendor-provided SVGs.

### Benefit and utility icons

Use **Solar Bold Duotone** through [Iconify](https://iconify.design/):

- Collection prefix: `solar`
- Preferred variant suffix: `-bold-duotone`
- Creator: 480 Design
- License: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- Color the primary shape `#a5b4fc` and the secondary shape with reduced opacity.

Choose the closest semantic icon from Solar before drawing anything. Keep icons optically centered
inside their accent circles, not merely centered by path bounds.

### Product and brand icons

Use **Simple Icons** through Iconify for company and product marks:

- Collection prefix: `simple-icons`
- Preserve the official path geometry.
- Recolor monochrome marks to the asset palette when the composition requires it.
- Keep product names in accessible text; never rely on the logo alone.

### Embedding

- Embed icon paths directly inside the final SVG.
- Do not reference Iconify, a CDN, remote fonts, or another network asset from the committed SVG.
- Do not commit downloaded PNG variants when an SVG exists.
- Record the icon IDs, creator, source, and license in a comment near the top of the containing SVG:
  `Solar icons by 480 Design, via Iconify, licensed CC BY 4.0:`
  `https://creativecommons.org/licenses/by/4.0/`.
- Simple Icons is released under CC0 1.0, but individual trademarks remain the property of their
  respective owners. Preserve that distinction in provenance comments.
- Keep brand icons grouped and transformed independently so their visual size can be tuned.

Iconify's API can optionally be used to inspect a source icon without adding a dependency. Confirm
the returned collection, icon name, and license before copying any geometry:

```sh
curl -fsSL "https://api.iconify.design/solar/<icon-name>-bold-duotone.svg"
curl -fsSL "https://api.iconify.design/simple-icons/<brand-name>.svg"
```

Copy the required vector geometry into the destination asset, then adapt fill, opacity, transform,
and alignment to the established visual system.

## SVG requirements

- Use a `viewBox`; do not depend on fixed pixel dimensions.
- Include `role="img"`, `aria-labelledby`, a concise `<title>`, and a complete `<desc>`.
- Keep the SVG self-contained and compatible with GitHub's SVG renderer.
- Do not use scripts, animation, external stylesheets, webfonts, remote `<image>` or `<use>` hrefs,
  or CSS that depends on browser-specific behavior.
- Inline styles and definitions. Avoid filters and masks unless they have been verified in the
  rendered GitHub README.
- Expect Geist to be unavailable on GitHub. Verify the system-font fallback does not clip, wrap, or
  break alignment.
- Use SVG `<text>` with the shared font stack. Do not use `<foreignObject>`.
- Use semantic group comments for major cards or regions.
- Use kebab-case filenames.
- Keep paths and transforms readable enough to adjust without regenerating the entire asset.
- Do not apply a CSS `transform` to an element that already has an SVG `transform` attribute; CSS wins
  and can move embedded icon paths out of position. Compose translation and scaling in the SVG
  attribute instead.
- Put emoji in a dedicated `<tspan>` using Apple Color Emoji, Segoe UI Emoji, and Noto Color Emoji
  fallbacks. Confirm the semantic text in a browser snapshot because raster tools may lack color emoji.
- Update README `alt` text whenever visible facts in an asset change.
- Standalone illustrations require `role`, `<title>`, and `<desc>`. Small CTA and logo SVGs may rely
  on the embedding README `<img>` alt text instead.

## Building an asset

1. Read the neighboring SVG that most closely matches the requested composition.
2. Confirm every public-facing fact against the current source before encoding it in text or alt
   text.
3. Establish the card grid and shared guides before adding content.
4. Select Solar Bold Duotone or Simple Icons assets and embed their vector paths.
5. Add copy using the existing typography classes and hierarchy.
6. Validate the XML and repository diff.
7. Render a 2× PNG to a temporary path.
8. Inspect the rendered pixels at full size and at approximate GitHub width.
9. Open the SVG or containing README in Orca's embedded browser when available. Reload after every
   material layout change; the browser's font metrics can expose overflow that `rsvg-convert` misses.
10. For compound lockups, use browser `getBBox()` measurements to confirm the combined bounds are
    centered inside their container.
11. Fix optical alignment, clipping, wrapping, and inconsistent padding before requesting review.

## Validation

Run from the repository root:

```sh
xmllint --noout profile/assets/<asset>.svg
git diff --check
rsvg-convert -w 1920 -o /private/tmp/<asset>.png profile/assets/<asset>.svg
rsvg-convert -w 900 -o /private/tmp/<asset>-github.png profile/assets/<asset>.svg
sips -g pixelWidth -g pixelHeight /private/tmp/<asset>.png
```

For multiple SVGs:

```sh
for asset in profile/assets/*.svg profile/assets/tech-stack/*.svg; do
  xmllint --noout "$asset" || exit 1
done

for reference in $(rg -o 'assets/[^" ]+' profile/README.md); do
  test -e "profile/${reference%%\?*}" || exit 1
done
```

The generated PNG is for review only. Keep it in `/private/tmp`; do not commit it.

## Boundaries

### Always

<always>

- Render and visually inspect every changed public-facing SVG.
- Preserve the existing style, palette, spacing rhythm, and typography.
- Use Solar Bold Duotone for utility icons and Simple Icons for brand marks.
- Keep icon geometry embedded and assets self-contained.
- Validate XML, Markdown references, and diff whitespace before committing.

</always>

### Ask first

<ask>

- Introducing another icon library or visual style.
- Changing an established asset's external aspect ratio after it is embedded in the README. New
  drafts may expand the canvas when content requires it, while preserving 16:9 when practical.
- Replacing an existing SVG with a raster asset.
- Removing an asset referenced by the profile README.

</ask>

### Never

<never>

- Commit temporary renders, downloaded PNG icon variants, or external asset URLs inside an SVG.
- Distort a logo by forcing both width and height against its native aspect ratio.
- Claim benefits, compensation, equipment, or interview details that have not been confirmed.
- Skip visual inspection because the SVG passes XML validation.

</never>
