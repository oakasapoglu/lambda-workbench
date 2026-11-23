# λ workbench

A small lambda calculus workbench for exploring reduction visually by clicking, not by typing raw terms.

It's one self-contained `index.html` in vanilla JS, with no frameworks, no build step and no network access. Open it in a browser and it works offline.

## Features

- Parser for `λx.body` and `\x.body`, multi-param `λx y.b`, and UPPERCASE or digit names as saved terms (`PLUS 2 3`).
- Capture-avoiding substitution, normal-order (leftmost-outermost) stepping with the next redex highlighted, and alpha-equivalence via de Bruijn indices.
- Three panes: Favorites, Expression and History. You build terms by clicking favorites and subterms.
- History aliases (`= 5`, `= TRUE`), with Church numerals 0–20 recognized automatically.
- Five themes: GitHub Dark, Tokyo Night, Monokai, Gruvbox and Gruvbox Soft Light.
- Collapsible side panes and four pane layouts.
- Toggle between showing saved names and their full λ definitions.
- Favorites are saved in the browser, with export and import as a plain text file.

## Keys

| Key | Action |
| --- | --- |
| Space | One reduction step |
| Shift+Space / Backspace | Undo one step |
| Enter | Evaluate to normal form (up to the step limit) |
| Esc / Delete | Clear the selection / remove the selected subterm |
| T | Next theme |
| F | Pane layout: both → focus → history only → favorites only |
| N | Names on/off |
| + − 0 | Zoom the expression / reset zoom |
| ? | Show all shortcuts |

Shift-click a favorite to apply the selected subterm to it.

## Tests

Run `runTests()` in the browser console, or click **Run tests** in the app.
