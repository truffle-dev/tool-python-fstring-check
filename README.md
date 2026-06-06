# tool-python-fstring-check

A single-page Python f-string compatibility checker. Paste code containing
f-strings, see which Python versions accept them.

Live: https://truffle.ghostwright.dev/public/tools/python-fstring-check/

## What it does

Lexes Python source for f-string literals (`f""`, `f''`, `f"""..."""`, `rf""`, `fr""`,
`bf` rejected, prefix combinations honored). For each expression part inside a
`{...}`, checks the four restrictions PEP 701 lifted in Python 3.12:

- Backslash anywhere in the expression region (including inside nested string literals).
- Same-quote as the f-string boundary, nested in the expression (`f'name is {d['name']}'`).
- Newline inside the expression (multi-line expression).
- Inline `#` comment inside the expression (outside any nested string).

Each finding includes line, column, snippet, and the rule it tripped. The
verdict table lists the two relevant Python eras: `3.6 – 3.11` (PEP 701
restrictions in effect) and `3.12+` (all four lifted).

## What it does not check

- Syntactic validity beyond f-string boundaries (won't tell you your tuple is malformed).
- Runtime behavior — the analyzer is static.
- Walrus expressions, named expressions, conversion flags, or format-spec correctness.
- 3.6 specifically — f-strings exist there but some edge cases (assignment expressions
  inside braces) were added later.

The tool answers one question: "will this f-string parse on Python <3.12?"

## Why it exists

I just fixed a Python 3.11 SyntaxError in a real project (WGDashboard#1290) where
an f-string contained `data.strip('\n')` — a backslash inside the expression,
which Python 3.12+ accepts but 3.11 rejects. The fix took five minutes; finding
the root cause took longer. A grep-by-eye for `f"..{..\\..}"` is the kind of
thing a one-page tool should do for me. So I built it.

## Shape

One HTML file. Inline CSS. Inline vanilla JS. No build step, no npm, no CDN,
no analytics, no trackers. Save the page and it works offline forever. The URL
is canonical and bookmarkable — paste once, share the link with the input
encoded in the hash.

## License

MIT. See LICENSE.
