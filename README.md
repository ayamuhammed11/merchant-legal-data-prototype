# Merchant Legal Data — UI prototype

A single-file HTML prototype for recording a merchant's legal document data as text, typed in manually by a back-office agent.

**[Open the prototype](https://ayamuhammed11.github.io/merchant-legal-data-prototype/)** (once GitHub Pages is enabled)

## What it does

It lives as a **Legal Data** sub-tab beside **All documents** on a merchant account. The page starts empty.

1. The agent clicks **Add document data** and picks a document type from a plain list of names.
2. That type gets its own card with the fields defined for it.
3. The agent types the values as text and saves. Everything stays editable afterwards.

## Behaviour

- **One card per document type** — independent header, field grid and save/cancel footer. Saving one card never touches another.
- **Fields come from the type definition**, so the form grows as new types are defined rather than being hardcoded per screen.
- **Validation** — per-field format rules (National ID = 14 digits, CR = 4–8 digits, Tax = 9 digits, IBAN = `EG` + 27 digits) plus expiry-date handling (expired / expiring within 60 days).
- **Conditional requirements** — VAT Registration Number only becomes required once VAT Status is set to *Registered*.
- **Cancel vs. discard** — on a card that was never saved the footer button reads **Cancel** and removes the card entirely; once it holds saved data it reads **Discard changes** and reverts the fields to their last saved values.
- **Provenance** — every value records who entered it and when, shown under the field. The source is tagged `Manual`, leaving room for an OCR pass to populate the same fields later without changing the screen.
- **Events** — a full audit trail at the bottom of the page: sections added or cancelled, and every value filled, edited or cleared, each expanding to show the old and new value per field.

## Running it

No build step, no dependencies. Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

Values persist to `localStorage` per merchant ID, so a refresh keeps your input. Clear the `kashier.legalData.*` key to start over.

## Notes

This is a design prototype — static HTML, CSS and vanilla JS in one file. There is no backend, no network calls, and every merchant detail in it is fabricated.
