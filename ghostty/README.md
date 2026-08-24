# Ayu Dark Pro (Ghostty theme)

A dark terminal theme built on Ayu's color palette, with a custom dark navy
background instead of Ayu's own near-black default.

`themes/ayu-dark-pro`:

```
background            = #151a20   # dark navy background
foreground            = #bdc7d3   # primary text, a light cool blue-grey
selection-background  = #2d3d55   # muted blue-grey used behind selected text
selection-foreground  = #bdc7d3   # matches foreground
cursor-color          = #8fa0b5   # muted slate blue used for the block cursor
cursor-text           = #151a20   # matches background, keeps the glyph under
                                  # the cursor legible

palette = 0=#2e3946    # ansiBlack, a dark neutral slate
palette = 1=#eb7077    # ansiRed
palette = 2=#61bc33    # ansiGreen
palette = 3=#f7b860    # ansiYellow
palette = 4=#57bdfe    # ansiBlue
palette = 5=#ff33ff    # ansiMagenta
palette = 6=#82d8c8    # ansiCyan
palette = 7=#c3cdd8    # ansiWhite, a light neutral blue-grey
palette = 8=#3d4b5c    # ansiBrightBlack, a mid neutral slate
palette = 9=#eb7077    # ansiBrightRed, same as ansiRed
palette = 10=#61bc33   # ansiBrightGreen, same as ansiGreen
palette = 11=#f7b860   # ansiBrightYellow, same as ansiYellow
palette = 12=#57bdfe   # ansiBrightBlue, same as ansiBlue
palette = 13=#ff33ff   # ansiBrightMagenta, same as ansiMagenta
palette = 14=#82d8c8   # ansiBrightCyan, same as ansiCyan
palette = 15=#d0d7e0   # ansiBrightWhite, a bright neutral blue-grey
```

The red, green, yellow, blue, magenta, and cyan slots are this theme's own
tuned hue palette (picked for readability and for distinguishing hues from each
other, including under red-green color vision), each used for both the normal
and bright slot since this theme only defines one shade per hue. Standard ANSI
has no dedicated orange slot, so this theme's separate orange accent doesn't
appear in the terminal palette. The neutrals (black, bright black, white,
bright white) are dark-navy-tinted greys that sit consistently between the
background and the brightest foreground.

## Installing

Ghostty only loads custom themes from `$XDG_CONFIG_HOME/ghostty/themes/`, i.e.
`~/.config/ghostty/themes/`, regardless of where your main `config` file itself
lives. On macOS the main config lives at
`~/Library/Application Support/com.mitchellh.ghostty/config`, but themes still
load from the `~/.config` path.

1. Copy `themes/ayu-dark-pro` to `~/.config/ghostty/themes/ayu-dark-pro`.
2. Add this line to your Ghostty `config`:
   ```
   theme = ayu-dark-pro
   ```
3. Restart Ghostty (not just reload config, a fresh theme needs a full restart
   to be picked up the first time).

## Regenerating from scratch

Use the hex values in the table above directly for each `palette` line, the
background, and the foreground. Derive the remaining neutrals (selection
background, cursor color, the neutral palette slots) as shades of the same
blue-grey hue at different lightness levels.
