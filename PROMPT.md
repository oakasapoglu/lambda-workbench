# Original prompt

The spec this workbench was built from, kept verbatim.

---

Build a small lambda calculus workbench as a single self-contained
index.html (vanilla JS, no frameworks, no build step, no network).
Goal: explore reduction visually by clicking, not by typing raw terms.

## Core model
- Terms: Var(name) | Lam(param, body) | App(fn, arg), as plain JS objects.
- Parser accepts `λx.body` and `\x.body`, multi-param `λx y.b`,
  left-associative application, parentheses, and UPPERCASE names or
  digits as references to saved terms (e.g. `PLUS 2 3`).
- Printer: minimal parentheses, `λ` notation; names expand only when
  needed for reduction.
- Capture-avoiding substitution (rename bound vars with fresh names).
- Normal-order (leftmost-outermost) beta reduction, one step at a time.
  Highlight the redex about to fire.
- Alpha-equivalence via conversion to de Bruijn indices.

## UI (three panes)
1. Favorites (left): named terms, grouped (Booleans, Numerals,
   Arithmetic, Combinators). Clicking one:
   - replaces the selected subterm, if one is selected;
   - otherwise applies the current expression to it (or sets it, if empty).
2. Expression (center): the current term, with every subterm clickable
   to select it (outline the selection; Esc clears). This lets me build
   `AND (NOT TRUE) FALSE` purely by clicking.
   A small text box can also accept a typed term, but it's optional.
3. History (right/below): every step, numbered, with an alias label
   when it matches a saved term (e.g. `= 5`, `= TRUE`).

## Controls (no prompts or dialogs during normal use)
- Space: one reduction step. Backspace: undo one step.
  (Ignore both while a text input has focus.)
- Eval button / Enter: reduce to normal form, stopping at STEP_LIMIT
  (a const, default 100, also editable in a small settings field).
  If the limit is hit, say so inline, e.g. "stopped after 100 steps".
- Save: inline name field + button stores the current term as a favorite.
- Delete a favorite with a small × on hover.

## Aliases
After each step and on eval, compare the term to all favorites up to
alpha-equivalence and show the matching name. Church numerals 0–20 are
recognized automatically even if not saved.

## Persistence
- Ship defaults embedded in the file: TRUE FALSE AND OR NOT IF,
  0–5, SUCC PRED PLUS MULT ISZERO, I K S, Y, OMEGA.
- Auto-save favorites to localStorage on every change; load on startup.
- Export/Import a plain, hand-editable text file, one definition per line:
    NAME = λx.term      # comments allowed
  Parse it strictly as data (no eval, no Function()). Report bad lines
  inline and skip them.

## Constraints
- One readable file. Clear separation between the
  pure core (parse, print, subst, step, alpha-eq) and the UI.
- The core must be pure functions with no DOM access.
- Include a `runTests()` function (callable from the console) that checks:
    PLUS 2 3 → 5;  MULT 2 3 → 6;  AND TRUE FALSE → FALSE;
    NOT (NOT TRUE) → TRUE;  λx.λy.x ≡α λa.λb.a;
    capture test: (λx.λy.x) y → λy'.y (not λy.y);
    OMEGA stops at STEP_LIMIT without hanging the page.
- Keyboard-friendly, readable monospace layout, works offline .
