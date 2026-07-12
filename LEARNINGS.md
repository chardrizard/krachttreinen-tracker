# Learnings

> Append-only failure log. When something fails, a fix doesn't work, or an assumption proves wrong, add an entry. Read this before related work. Promote durable, reusable lessons to a `memory/` file and link it.

Entry format:

```
## YYYY-MM-DD — short title
- **What failed:** …
- **Why:** …
- **Rule going forward:** …
- **Promoted to memory:** [[memory-slug]]  (only if durable)
```

---

## 2026-07-12 — Screen state must target the real shell boundary
- **What failed:** The redesigned Select screen still showed part of the bottom navigation during the first browser pass.
- **Why:** Navigation lives outside `#app`, but the initial CSS tried to hide it through an `#app[data-screen]` descendant selector.
- **Rule going forward:** Put navigation state on `body` when fixed chrome sits outside the application content wrapper, and verify both the visual render and accessibility tree after navigation.
- **Promoted to memory:** [[performance-ledger-design]]

## 2026-07-12 — Derive exercise marks from semantic word boundaries
- **What failed:** Slash-separated exercise names produced marks such as `P/`.
- **Why:** The initial abbreviation helper split only on whitespace and treated punctuation as a word.
- **Rule going forward:** Split exercise names on whitespace and separators, then discard tokens without letters or numbers before deriving initials.
- **Promoted to memory:** [[performance-ledger-design]]

## 2026-07-12 — Keep viewport chrome fixed in desktop preview
- **What failed:** The bottom navigation floated above the bottom edge at preview widths of 600px and wider.
- **Why:** A desktop-preview rule changed the navigation to `position: absolute`, while the navigation sits outside the centred `#app` shell. Its containing block was therefore the document, not the app shell or viewport.
- **Rule going forward:** Desktop preview may constrain and decorate the app shell, but viewport chrome must keep its mobile positioning model unless it is structurally moved inside that shell.
- **Promoted to memory:** [[performance-ledger-design]]

## 2026-07-12 — Generated initials are not exercise identity
- **What failed:** Two-letter marks such as `FB`, `ID`, and `CF` created visual rhythm but required users to decode arbitrary abbreviations.
- **Why:** The marks compressed names without adding recognition. A controlled comparison also showed that category and equipment glyphs often repeat information already present in headings or exercise names.
- **Rule going forward:** Lead with the full exercise name. Use quiet equipment/category metadata only when it adds explicit information, and do not invent equipment from ambiguous exercise names.
- **Promoted to memory:** [[performance-ledger-design]]
