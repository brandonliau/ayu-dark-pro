# Ayu Dark Pro

Ayu's colors blended into One Dark Pro Darker's syntax-highlighting roles and
UI structure.

This document specifies every value and rule needed to build the theme
directly from One Dark Pro Darker's theme JSON and Ayu's color palette, with
no other inputs required.

## Sources

- **One Dark Pro Darker**: the theme JSON from the *One Dark Pro* VS Code
  extension (publisher `zhuangtongfa.Material-theme`). Its `theme.colors`,
  `theme.tokenColors`, and `theme.semanticTokenColors` supply every UI key,
  every token/scope rule, and the per-family "primary" role colors referenced
  below.
- **Ayu**: the Ayu Dark iTerm2 color scheme (16 ANSI colors, plus background,
  foreground, and cursor), cross-checked against the published Ayu Dark color
  palette (`ayu-colors`). Every Ayu hex value used below comes from that
  source.

## Backgrounds

This theme uses two custom background tiers, not derived from either source:

| Tier | Value | Used for |
|---|---|---|
| Editor | `#151A20` | `editor.background`, `activityBar.background`, `tab.activeBackground`, `editorGroup.emptyBackground`, `terminal.background`, and every key where One Dark Pro Darker uses its own `#23272e` |
| Chrome | `#12171D` | `sideBar.background`, `statusBar.background`, `dropdown.background`, and every key where One Dark Pro Darker uses its own `#1e2227` |

## Neutral / grey UI colors

Every UI color that is not part of a hue family (see below) and is not a
`terminal.ansi*` hue color, borders, hovers, line numbers, foregrounds,
scrollbars, etc., is built from One Dark Pro Darker's value for that key
combined with this theme's backgrounds:

1. Take One Dark Pro Darker's color `C` for the key, and convert to HSL.
2. One Dark Pro Darker uses two background tiers of its own, `#23272e` and
   `#1e2227`. Pick whichever is closer to `C`'s lightness. Call it `A`, with
   lightness `L(A)`.
3. `delta = L(C) - L(A)`
4. This theme's corresponding tier (`#151A20` for `#23272e`, `#12171D` for
   `#1e2227`) is `A'`. Take `A'`'s own hue and saturation.
5. `L' = L(A') + delta`, clamped to `[0, 1]`.
6. Result: `HSL(H(A'), S(A'), L')`.

Every neutral color keeps the same brightness offset from its background tier
that One Dark Pro Darker gives it, expressed with this theme's own background
hue and saturation, so the whole grey ramp reads as one consistent cool
blue-grey. A key whose One Dark Pro Darker value is exactly one of its two
background tiers maps exactly onto this theme's corresponding tier (delta =
0).

Two exceptions are carried over from One Dark Pro Darker exactly as they are,
not run through this rule: `#ffffff` (and its alpha variants, used for
cursor/overlay effects) and fully transparent `#00000000`-style borders.

## Hue-family token colors

One Dark Pro Darker's syntax and UI colors fall into 7 hue families. Each
family has one "primary" role color in One Dark Pro Darker (for example red's
primary is the tag/variable color `#e06c75`), and every other color in that
family (darker variants, error reds, terminal ANSI colors, etc.) sits at some
HSL offset from that primary within One Dark Pro Darker's own palette.

**Anchor.** For each family, take Ayu's color for that same role and One Dark
Pro Darker's primary for it, and blend them 70% Ayu, 30% One Dark Pro Darker
per RGB channel: `anchor = 0.70 * ayu + 0.30 * odpd`. This keeps the token
colors from reading as pure, maximally-saturated Ayu, closer to One Dark Pro
Darker's softer feel.

**Per-family tuning**, applied on top of the anchor (chosen by eye, comparing
hues directly against each other for readability and for distinguishing hues
under red-green color vision):

- **Green**: hue set to 100 degrees (a purer green, less yellow-green).
- **Orange**: hue set to 15 degrees (a true, redder orange, separated from
  green's hue), lightness raised 2% on top of that.
- **Purple**: hue set to 300 degrees (a magenta-leaning purple, separated from
  blue's hue), lightness set to 60%.
- **Blue**: saturation raised to a fully clamped 100%.
- **Red, yellow, cyan**: the 70/30 blend value, no further tuning.

**Family anchors** (One Dark Pro Darker's primary role color, and this
theme's anchor for the same role):

| Family | One Dark Pro Darker | Ayu Dark Pro | Role |
|---|---|---|---|
| Red | `#e06c75` | `#eb7077` | tags, HTML/JSX attribute names, invalid/error tokens |
| Orange | `#d19a66` | `#f06436` | numeric literals, constants |
| Yellow | `#e5c07b` | `#f7b860` | class/type names |
| Green | `#98c379` | `#61bc33` | strings |
| Cyan | `#56b6c2` | `#82d8c8` | regex, some operators/types |
| Blue | `#61afef` | `#57bdfe` | function/method names |
| Purple | `#c678dd` | `#ff33ff` | keywords, storage modifiers |

**Every other member of a family.** For any color `C` in One Dark Pro Darker
classified into family `F` (see the table below) that is not the family's
primary:

```
oh, os, ol = HSL(F's One Dark Pro Darker primary)
ah, as, al = HSL(F's Ayu Dark Pro anchor, from the table above)
h,  s,  l  = HSL(C)

dH = circular_shortest_signed_difference(h, oh)   # in [-180, 180) degrees
dS = s - os
dL = l - ol

C' = HSL(ah + dH, clamp(as + dS), clamp(al + dL))
```

`C` keeps its own hue, saturation, and lightness offset from One Dark Pro
Darker's family primary, re-anchored onto this theme's family anchor. A color
that One Dark Pro Darker makes darker or more saturated than its family's
primary stays proportionally darker or more saturated here too.

**Classification** (every One Dark Pro Darker color that goes through this
rule, by family):

| Family | One Dark Pro Darker colors |
|---|---|
| Red | `#e06c75`, `#be5046`, `#c24038`, `#f44747`, `#9a353d` |
| Orange | `#d19a66`, `#cc6633` |
| Yellow | `#e5c07b`, `#948b60`, `#ffd33d` |
| Green | `#98c379`, `#109868` |
| Cyan | `#56b6c2`, `#00809b` |
| Blue | `#61afef`, `#528bff`, `#4d78cc`, `#677696`, `#d2e0ff` |
| Purple | `#c678dd`, `#29244b`\* |

\* `#29244b` (`peekViewEditor.matchHighlightBackground`) is a hand override,
see below.

Every other saturated, hue-bearing color in One Dark Pro Darker that is not
in this table is a `terminal.ansi*` color and is handled by the terminal
mapping below, or is a neutral color handled by the rule above.

## Terminal ANSI colors

`terminal.ansi*` colors are not run through the hue-family formula. They map
directly from One Dark Pro Darker's value onto Ayu's own 16-color ANSI
palette, so the integrated terminal always shows pure Ayu colors regardless
of how the editor's syntax colors are tuned:

| One Dark Pro Darker | VS Code key | Ayu Dark Pro |
|---|---|---|
| `#e05561` | `terminal.ansiRed` | `#ea6c73` |
| `#ff616e` | `terminal.ansiBrightRed` | `#f07178` |
| `#8cc265` | `terminal.ansiGreen` | `#7fd962` |
| `#a5e075` | `terminal.ansiBrightGreen` | `#aad94c` |
| `#d18f52` | `terminal.ansiYellow` | `#f9af4f` |
| `#f0a45d` | `terminal.ansiBrightYellow` | `#ffb454` |
| `#4aa5f0` | `terminal.ansiBlue` | `#53bdfa` |
| `#4dc4ff` | `terminal.ansiBrightBlue` | `#59c2ff` |
| `#c162de` | `terminal.ansiMagenta` | `#cda1fa` |
| `#de73ff` | `terminal.ansiBrightMagenta` | `#d2a6ff` |
| `#42b3c2` | `terminal.ansiCyan` | `#90e1c6` |
| `#4cd1e0` | `terminal.ansiBrightCyan` | `#95e6cb` |

`terminal.ansiBlack`/`ansiBrightBlack`/`ansiWhite`/`ansiBrightWhite` are
neutral greys, handled by the neutral rule above.

## Hand overrides

Two One Dark Pro Darker colors are pinned to a fixed value instead of the
neutral or hue-family formula, because those formulas produce a poor result
for these two specifically:

- **`#29244b`** (`peekViewEditor.matchHighlightBackground`) becomes
  **`#3a2253`**. Its hue sits far enough from the purple family's primary
  that the hue-family formula lands in blue instead of purple. Picked by hand
  for a dark, plausible purple/plum, at roughly the same darkness and
  subtlety as One Dark Pro Darker's own value.
- **`#abb2bf`** (One Dark Pro Darker's single shared "primary text" color,
  used for `editor.foreground`, `sideBar.foreground`, `terminal.foreground`,
  `input.foreground`, `menu.foreground`, `descriptionForeground`, and the
  many "plain text / punctuation / default variable" token scopes that share
  this color) becomes **`#97a6ba`**. This is a fixed pin, not a formula. Pick
  a different hex here for a different primary text brightness.

## Colors added outright

One Dark Pro Darker does not set `breadcrumb.*` at all. This theme adds these
keys directly:

| Key | Value | Note |
|---|---|---|
| `breadcrumb.background` | same as `editor.background` (`#151A20`) | |
| `breadcrumb.foreground` | `#5c708a` | idle breadcrumb segment text |
| `breadcrumb.focusForeground` | `#8fa0b5` | hovered/focused segment text |
| `breadcrumb.activeSelectionForeground` | `#8fa0b5` | same as focus |
| `breadcrumbPicker.background` | same as `editorWidget.background` | |

## Tab border

Two deliberate rules for the active tab's borders, layered on top of the
neutral rule's result for these two keys:

- **`tab.activeBorder`** (the border on the bottom of the active tab, between
  the tab and the editor) is set equal to **`tab.border`**, so there is no
  separate line there. Only the top bar and the background-shade difference
  between `tab.activeBackground` and `tab.inactiveBackground` mark the
  active tab.
- **`tab.activeBorderTop`** (the bar across the top of the active tab) is
  `tab.activeBackground`'s own HSL, with lightness raised 18%, same hue and
  saturation, clamped to 1.0.

## Reference

The complete set of values needed to build the theme, in one place:

```
Backgrounds:      editor tier #151A20   |   chrome tier #12171D
Family anchors:   red #eb7077  orange #f06436  yellow #f7b860
                  green #61bc33  cyan #82d8c8  blue #57bdfe  purple #ff33ff
Hand overrides:    #29244b -> #3a2253      (peek-view match highlight)
                    #abb2bf -> #97a6ba      (primary text, everywhere)
Breadcrumb:        idle #5c708a   hover/focus #8fa0b5
Tab:               activeBorder = tab.border   |   activeBorderTop = activeBackground +18% L
```

## Building the theme

1. Take a copy of the "One Dark Pro Darker" theme JSON (from the *One Dark
   Pro* VS Code extension) as the base for `theme.colors`, `theme.tokenColors`,
   and `theme.semanticTokenColors`.
2. Apply the neutral rule to every color that is not part of a hue family and
   not a `terminal.ansi*` hue color.
3. Apply the hue-family formula to every color in the classification table,
   across `colors`, `tokenColors`, and `semanticTokenColors`.
4. Apply the terminal ANSI mapping to the 12 hue-bearing `terminal.ansi*`
   colors.
5. Apply the two hand overrides.
6. Add the breadcrumb colors.
7. Apply the tab border rules.

Every Ayu value the formulas above need is already listed in this document's
tables. Only One Dark Pro Darker's theme JSON needs to be supplied separately,
since its full set of UI keys, token/scope rules, and semantic rules is not
reproduced here. With that one file, this document is sufficient on its own
to build the theme.
