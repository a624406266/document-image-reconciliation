# PDF-to-Excel reconciliation

## Prerequisites

Require both the reconciliation PDF and the Excel counterpart. If only the PDF is available, ask the user for the Excel file and stop.

Use the PDF skill for extraction and visual page review, and the Spreadsheets skill for read-only workbook inspection. Treat both files as immutable sources unless the user separately requests an edit.

## Extract and verify the PDF

- Inspect PDF metadata and page count.
- Render and visually inspect every relevant page.
- Extract tables and text, but do not rely on extraction alone for layout or page-continuation totals.
- Join totals that are visibly split across pages, such as an integer portion on one page and decimal digits on the next.
- Capture statement date, document number, supplier name, reconciliation period, row count, and printed total.

## Comparison levels

Compare at least:

1. statement metadata: date, document number, supplier, and period;
2. document coverage: row count, business dates, delivery/receipt/order identifiers, and document grouping;
3. line items: customer PO/order, purchase contract/order, material code, item name, unit, quantity, tax-inclusive unit price, and amount;
4. summaries: per-document or per-date quantity and amount, then grand totals.

Use a documented stable key. Prefer an explicit shared business document number. If the files contain different document types, use a composite key such as date + material code + customer PO/order, and report that the document-number relationship could not be directly verified. When duplicate keys exist, compare as a multiset and include quantity/amount in the matching logic.

Apply the same safe normalization rules as the OCR workflow. Presentation-only equivalents such as `千克` versus `KG`, comma-separated numbers, or reordered rows are not mismatches when their business meaning is identical. Preserve and report every identifier difference, including leading zeros.

## Classify findings

- **Confirmed mismatch:** both sources contain comparable values and they differ.
- **Information gap:** a field exists in only one source or the schemas contain different document types.
- **Presentation-only difference:** values are semantically equal after permitted normalization.
- **Match:** comparable values agree.

Do not label an information gap as a confirmed accounting error. Do not declare the reconciliation correct while a comparable field mismatch remains unresolved.

## Comparison report

Create a new Markdown report named `对账单对比信息差.md` unless the user requests another name. Include:

- source filenames and inspected ranges/pages;
- statement date, document number, supplier, and reconciliation period;
- overall conclusion;
- confirmed mismatches with both values, scope, and recommended verification owner;
- information gaps and non-comparable identifiers;
- per-document or per-date row, quantity, and amount summaries;
- grand totals;
- normalization and matching rules used;
- execution date and checks not performed.

If no material problem is found, explicitly write: `对账无误`, together with the statement date, document number, supplier name, and reconciliation period.
