# NCS Colour Explorer

An interactive map of the Natural Colour System (NCS) notation space. Choose a blackness slice, explore hue and chromaticness, inspect a notation, and copy an approximate screen color.

[Open the live NCS Colour Explorer](https://vire.github.io/ncs-color-picker/)

> [!IMPORTANT]
> NCS describes perceived surface colors. RGB, HEX, HSL, CMYK, and RAL use different models or fixed collections. Values shown by this project are useful screen approximations, not certified color specifications. Confirm production decisions with official physical samples under controlled lighting.

## How NCS works

NCS organizes colors by their perceived resemblance to six elementary colors:

- White (`W`)
- Black (`S`)
- Yellow (`Y`)
- Red (`R`)
- Blue (`B`)
- Green (`G`)

White and black are achromatic. Yellow, red, blue, and green form the hue circle. A chromatic NCS notation combines a **nuance** with a **hue**.

### Reading `NCS S 4040-R20B`

| Part | Meaning |
| --- | --- |
| `NCS` | Natural Colour System |
| `S` | A standardized NCS color from NCS Edition 2 |
| first `40` | 40% perceived blackness |
| second `40` | 40% perceived chromaticness |
| `R20B` | A red hue with 20% perceived resemblance to blue |

Whiteness is implicit:

```text
whiteness = 100 - blackness - chromaticness
          = 100 - 40 - 40
          = 20%
```

The nuance is therefore 40% blackness, 40% chromaticness, and 20% whiteness. The hue `R20B` sits between red and blue, with 80% red resemblance and 20% blue resemblance.

Pure elementary hues are written `Y`, `R`, `B`, or `G`. The explorer writes intermediate hue percentages as two digits from `01` through `99`, such as `Y30R` or `B70G`. Neutral colors have no chromatic hue and use `N`, for example `NCS S 4000-N`.

The notation describes relationships in the full NCS color space. The official standard collection is a selected set of 2,050 physical colors, so a syntactically valid point in the color space is not necessarily one of the NCS 2050 standard samples. The [official NCS Navigator documentation](https://plus-support.ncscolour.com/help/using-the-ncs-navigator) explains hue and nuance navigation across those standard colors.

## Translating NCS to RGB, HEX, HSL, and CMYK

There is no universal arithmetic formula that turns an NCS notation into one exact RGB or CMYK value. NCS is based on perceived surface-color attributes, while RGB describes emitted display light and CMYK describes a device and process-dependent print mixture.

For controlled conversion:

1. Start with authoritative colorimetric data for the specific NCS standard sample, normally CIE XYZ or CIELAB under a stated illuminant and observer.
2. Convert that data into the target RGB space, such as sRGB, using the correct white-point adaptation.
3. Apply gamut mapping when the NCS color is outside the target display gamut.
4. Encode the resulting sRGB values as RGB, HEX, or HSL.
5. For CMYK, convert through the ICC profile for the exact press, ink, and paper combination instead of using a generic formula.

CSS RGB and HEX colors are defined in sRGB. The [W3C CSS Color specification](https://www.w3.org/TR/css-color-3/#rgb-color) also notes that colors outside a device gamut must be clipped or mapped. NCS offers official closest-color and digital-value tools through [NCS+ conversion](https://plus-support.ncscolour.com/converting-to-closest-ncs).

This explorer includes a local copy of the MIT-licensed [`ncs-color` 1.0.1](https://www.npmjs.com/package/ncs-color/v/1.0.1) package for an RGB approximation. If that script cannot run, it uses an HSL-based fallback. Neither path uses official NCS colorimetric data, so both are previews only. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for the dependency license.

## Translating between NCS and RAL

NCS and RAL codes are not interchangeable:

- NCS notation describes perceptual attributes and position in the NCS color space.
- RAL CLASSIC is a historically assembled collection whose four-digit identifiers do not encode color coordinates.
- RAL DESIGN SYSTEM plus is systematic and based on CIELAB, but it remains its own finite collection of standardized colors.

A defensible NCS-to-RAL workflow is a nearest-color search, not a direct formula:

1. Choose the target RAL collection and finish, such as RAL CLASSIC semi-matt or RAL DESIGN SYSTEM plus.
2. Obtain licensed CIELAB data for both the NCS color and every candidate in that RAL collection under compatible measurement conditions.
3. Compute a perceptual difference such as CIEDE2000 (`Delta E 00`).
4. Return the nearest candidate together with its measured difference, source data, illuminant, observer, and finish.
5. Approve the match against physical NCS and RAL samples in the intended lighting and material context.

RAL recommends CIELAB values for digital work and warns that on-screen representations are approximations. See the [RAL Digital Colour Library guidance](https://www.ral-farben.de/en/ral-digital-colour-library) and the description of the [CIELAB-based RAL DESIGN SYSTEM plus](https://www.ral-farben.de/en/ral-design-system-plus).

The explorer deliberately does not display RAL matches because it does not bundle licensed reference data from either standard.

## Using the explorer

- Move **Blackness** to choose a two-dimensional slice through the NCS space.
- Move outward from the center to increase chromaticness.
- Move around the circle to change hue through `R`, `B`, `G`, and `Y`.
- Change the resolution controls to show more or fewer sample points.
- Enter a notation such as `S 4040-R20B` to jump directly to it.
- Use the detail controls to adjust the selected point and copy its screen approximation.
- Press `[` or `]` when focus is outside a form control to collapse the left or right panel.

The generated map covers regular steps through the notation space. It is not a catalog of official NCS 2050 chips.

## Run locally

The site has no build step. Open `index.html` directly, or serve the directory so Clipboard API behavior and browser security rules match a hosted page:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

## Project scope and legal note

This is an independent educational project. It is not affiliated with, endorsed by, or certified by NCS Colour AB or RAL gGmbH. NCS and RAL names and marks belong to their respective owners. Do not use this tool as the sole basis for paint formulation, manufacturing, purchasing, quality control, or contractual color approval.

The project source is licensed under the [MIT License](LICENSE).
