# Ayu Dark Pro

Ayu's colors blended into One Dark Pro Darker's syntax-highlighting roles and
UI structure, packaged as a VS Code theme and a matching Ghostty terminal
theme.

```
custom-theme/
├── vscode/    the VS Code extension (theme, plus its own README)
└── ghostty/   the Ghostty theme (theme, plus its own README)
```

Each subfolder's README is a complete, standalone reference for that app: how
to install it, every color value and rule used to build it, and how to
rebuild it from scratch. This file is just the quickstart for getting both
running on a machine.

## VS Code

1. Create a folder named `local.ayu-dark-pro-1.0.0` inside your VS Code
   extensions folder:
   - macOS/Linux: `~/.vscode/extensions/`
2. Copy `vscode/package.json`, `vscode/README.md`, and
   `vscode/themes/` into it
3. Fully quit and reopen VS Code
4. `Cmd/Ctrl+K Cmd/Ctrl+T`, select "Ayu Dark Pro"

Full details, troubleshooting, and the theme's build spec: `vscode/README.md`

## Ghostty

1. Copy `ghostty/themes/ayu-dark-pro` to
   `~/.config/ghostty/themes/ayu-dark-pro`
2. Add this line to your Ghostty `config`:
   ```
   theme = ayu-dark-pro
   ```
3. Fully restart Ghostty

Full details and the theme's value reference: `ghostty/README.md`
