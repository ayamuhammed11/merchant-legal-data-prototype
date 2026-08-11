# Merchant Document Info — UI prototype

A single-file HTML prototype for recording a merchant's document information as text, typed in manually by a back-office agent.

**[Open the prototype](https://ayamuhammed11.github.io/merchant-legal-data-prototype/)**

## What it does

It lives as a **Document Info** sub-tab beside **All documents** on a merchant account, and reads as a work queue.

1. The page shows one card per document the merchant has uploaded — there is no "add" step, because what exists is decided by the uploads.
2. The agent opens a card and types its values as text.
3. Saving writes them to the account and closes the card. Everything stays editable afterwards.

## Behaviour

- **Cards are derived from the uploads** — a document type with no uploaded file gets no card, and a new upload produces one automatically.
- **Accordion, one card open at a time** — opening a second card closes the first, so the queue stays visible and the page never becomes an endless form.
- **Collapsing is Cancel** — anything typed and not saved is dropped rather than kept silently. Saving is always explicit.
- **Status at a glance** — a dot per row: grey untouched, amber partially filled, red a value that fails validation or has expired, green complete. The header counts how many uploaded documents are fully recorded.
- **Fields come from the type definition**, so the form grows as new types are defined rather than being hardcoded per screen.
- **Validation** — per-field format rules (National ID = 14 digits, Passport = alphanumeric 6–12, CR = numeric, Tax = 9 digits) plus expiry-date handling (expired / expiring within 60 days).
- **Save is blocked unless the card is both well-formed and complete** — every entered value must match its expected format, and every required field must be filled, before Save enables. The guard is re-checked on Save itself, so nothing incomplete or malformed can be written however Save was reached. An expired date does not block saving — it is a true fact about the document, just flagged.
- **Required fields don't shame you on open** — a freshly opened card never shows red; a required field only flags once you've actually reached it (tabbed past it empty, or tried to save). The Save button itself stays disabled the whole time regardless.
- **Namespaced field keys** — several keys (`full_name`, `date_of_birth`, `gender`, `nationality`, `issue_date`) appear on more than one document type, so values are stored as `<type>.<key>` to keep them independent.
- **Conditional requirements** — VAT Registration Number only becomes required once VAT Status is set to *Registered*, and it reacts live to what's currently typed, not just the last saved value.
- **Events** — a full audit trail at the bottom of the page: every value filled, edited or cleared, each expanding to show the old and new value per field.
- **Simulate an empty account** — a prototype-only toggle in the header swaps between the seeded uploads and an account with nothing uploaded, so both states are easy to demo without a second file.

Document review state (approved / pending / rejected) is deliberately out of scope here — this tab is only about keying the values off a document that exists.

## Running it

No build step, no dependencies. Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

Values persist to `localStorage` per merchant ID, so a refresh keeps your input. Clear the `kashier.legalData.*` key to start over.

## Notes

This is a design prototype — static HTML, CSS and vanilla JS in one file. There is no backend, no network calls, and every merchant detail in it is fabricated.
