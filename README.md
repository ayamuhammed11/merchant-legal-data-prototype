# Merchant Legal Data — UI prototype

A single-file HTML prototype for capturing a merchant's legal document data as text, entered manually by a back-office agent.

**[Open the prototype](https://ayamuhammed11.github.io/merchant-legal-data-prototype/)** (once GitHub Pages is enabled)

## What it does

The agent picks a document type, then types its values into text fields. Everything is saved against the merchant account and stays editable.

- **Add document data** — choose which document type to record from a list of types not yet added
- **Per-type form** — fields come from that type's definition, so the form grows as new types are defined
- **Validation** — per-field format rules (National ID = 14 digits, CR = 4–8 digits, Tax = 9 digits, IBAN = `EG` + 27 digits) plus expiry-date handling
- **Conditional requirements** — e.g. VAT Registration Number becomes required only once VAT Status is set to *Registered*
- **History** — per-document-type audit of what was filled, edited or cleared, by whom and when
- **Provenance** — every value stores its source (`Manual` today; an OCR pass can populate the same fields later without changing the screen)

## Running it

No build step, no dependencies. Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

Values persist to `localStorage` per merchant ID, so a refresh keeps your input. The page ships with seeded demo data for a fictional merchant; clear the `kashier.legalData.*` key to start empty.

## Notes

This is a design prototype — static HTML, CSS and vanilla JS in one file. There is no backend, no network calls, and all merchant data in it is fabricated.
