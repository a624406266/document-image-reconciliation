# Three-pass image recognition and Excel entry

## Recognition record

For each image, extract the same structured record in each pass:

- source image filename and document/page identifier;
- document title, issuing party, receiving party, addresses, document number, and document date;
- table headers and every line item in source order;
- identifiers, item names, units, ordered quantity, delivered quantity, unit price, amount, and remarks;
- printed totals and subtotals;
- handwritten prices, braces, corrections, signatures, stamps, and margin annotations;
- unreadable or partially visible text marked explicitly as unresolved.

Do not silently omit content merely because the current Excel template has no matching column.

## Independence of the three passes

- Start each pass from the original image, not a transcription made in another pass.
- Recheck orientation, crop, resolution, and zoom independently in every pass.
- Do not copy identifiers or quantities from a prior pass.
- Keep three separate scratch outputs until comparison is complete.
- A calculation check may expose a recognition error, but it must not be used to force a value to match an expected total.

The three passes may be completed by the same model in isolated passes. Do not require subagents or alternate OCR services unless the user explicitly requests them and they are available.

## Permitted normalization for consensus comparison

Normalize only representation differences that cannot change business meaning:

- trim surrounding whitespace and line-wrap artifacts;
- normalize full-width/half-width punctuation;
- normalize thousands separators in numeric values;
- normalize dates to `YYYY-MM-DD` after all date components are clearly recognized;
- normalize obvious unit aliases such as `千克` and `KG`;
- normalize multiplication symbols such as `x`, `X`, `*`, and `×` only in packaging remarks.

Do not normalize or repair identifiers beyond removing accidental layout line breaks. Preserve leading zeros, hyphens, underscores, suffixes, and letter case when case may be meaningful. Never silently convert ambiguous pairs such as `0/O`, `1/I`, `5/S`, or `8/B`.

## Consensus gate

Compare:

- document and line counts;
- every header value;
- every identifier and description;
- every quantity, price, amount, and remark;
- printed totals and handwritten annotations.

Three-way agreement is required. A field that is visibly blank in all three passes counts as an agreed source absence and must be logged. If any pass differs or any visible text remains illegible:

1. do not write to Excel;
2. log the source image, field/row, pass 1 value, pass 2 value, and pass 3 value;
3. show a focused crop or describe the exact location when useful;
4. ask the user for the authoritative value;
5. record the user's confirmed value and resume only for the resolved field.

Do not choose the majority value automatically.

## Template mapping gate

Before writing:

1. inspect workbook sheets, used ranges, formulas, merges, number formats, and relevant styles;
2. render the target sheet;
3. map source fields to existing headers by business meaning;
4. identify source fields with no destination and template fields with no source value;
5. ask the user about any ambiguous mapping.

If no workbook was supplied and the source is compatible with a conventional reconciliation statement, copy the bundled generic template and use [template-mapping.md](template-mapping.md). A supplied workbook takes priority over the bundled template.

Unless requested, do not add columns, merge cells, overwrite formulas, or fill missing source fields from inference. Put unmapped information in the Markdown log.

## Write and verify

- Work on a new copy, never the supplied original.
- When using the bundled template, copy `assets/generic-reconciliation-template.xlsx` to the task output directory and write only to that copy.
- Write line items only after all source images have passed consensus.
- Use formulas for derived amounts and totals when compatible with the template.
- Reconcile each document subtotal and the grand total to the recognized source.
- Inspect the final data range and formula cells, scan for formula errors, and render all edited sheets.
- If verification exposes a source or mapping discrepancy, stop, log it, and ask the user rather than silently revising recognized data.
