# Merchant Document Info — UI prototype

A single-file HTML prototype for a merchant's Documents tab, where a back-office agent records a document's data as text alongside reviewing the document itself.

**[Open the prototype](https://ayamuhammed11.github.io/merchant-legal-data-prototype/)**

## What it does

1. **Required Documents** is a table — one row per document type the merchant has uploaded. There is no "add" step: what exists is decided entirely by the uploads.
2. Clicking a row opens a slide-over drawer for that document, with three tabs: **Details**, **Document info fields**, and **Audit Logs**.
3. **Details** is read-only — a preview placeholder and the document's basic details (type, file name, uploader, upload date) listed one per row.
4. **Document info fields** is where the agent types that document's data as text — the core of the prototype — with its own Save/Cancel footer.
5. Saving writes the values to the account and keeps the drawer open, refreshed, on that same tab. **Audit Logs** shows every change made to that document, and only that document.

## Behaviour

- **Rows are derived from the uploads** — a document type with no uploaded file gets no row, and a new upload would produce one automatically.
- **One drawer at a time.** Closing it (✕, clicking the backdrop, Escape, or Cancel) discards anything typed and not saved — the drawer re-renders from what's stored the next time it opens.
- **The Save/Cancel footer follows the active tab** — it only shows on Document info fields, since Details and Audit Logs have nothing to save.
- **Fields come from the type definition**, so the form grows as new types are defined rather than being hardcoded per screen.
- **Validation** — per-field format rules (National ID = 14 digits, Passport = alphanumeric 6–12, CR = numeric, Tax = 9 digits) plus expiry-date handling (expired / expiring within 60 days).
- **Save is blocked unless the document is both well-formed and complete** — every entered value must match its expected format, and every required field must be filled, before Save enables. The guard is re-checked on Save itself, so nothing incomplete or malformed can be written however Save was reached. An expired date does not block saving — it is a true fact about the document, just flagged.
- **Required fields don't shame you on open** — a freshly opened drawer never shows red; a required field only flags once you've actually reached it (tabbed past it empty, or tried to save). The Save button stays disabled the whole time regardless.
- **Namespaced field keys** — several keys (`full_name`, `date_of_birth`, `gender`, `nationality`, `issue_date`) appear on more than one document type, so values are stored as `<type>.<key>` to keep them independent.
- **Conditional requirements** — VAT Registration Number only becomes required once VAT Status is set to *Registered*, and it reacts live to what's currently typed, not just the last saved value.
- **Audit Logs are per document**, not a single global feed — every value filled, edited or cleared, each expanding to show the old and new value per field.
- **Simulate an empty account** — a prototype-only toggle next to "Download all" swaps between the seeded uploads and an account with nothing uploaded, so both states are easy to demo without a second file.

Document review state (approved / pending / rejected) is deliberately out of scope — every row shows Approved, and this prototype is only about keying the values off a document that already exists.

## Running it

No build step, no dependencies. Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

Values persist to `localStorage` per merchant ID, so a refresh keeps your input. Clear the `kashier.legalData.*` key to start over.

## Notes

This is a design prototype — static HTML, CSS and vanilla JS in one file. There is no backend, no network calls, and every merchant detail in it is fabricated.
