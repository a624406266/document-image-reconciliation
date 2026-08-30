# Bundled generic reconciliation template

## Scope and precedence

- Asset: `assets/generic-reconciliation-template.xlsx`
- Template version: `1.0`
- Worksheet: `对账单`
- Intended for conventional supplier reconciliation statements and delivery-note summaries with up to 20 line items.
- A user-supplied workbook always takes priority. Do not copy fields from this mapping into a different workbook by cell address; inspect that workbook independently.
- If the document needs more than 20 rows, a materially different schema, multiple tax rates, multiple currencies, or columns not represented here, stop and ask the user before changing the layout.

Copy the asset to the task output directory before writing. Never edit the bundled asset in place.

## Metadata mapping

| Source meaning | Destination | Write rule |
| --- | --- | --- |
| Supplier/company name | `对账单!C1` | Write only when the issuing/supplier party is unambiguous. |
| Supplier/company address | `对账单!C2` | Write only when printed on the source. |
| Issuing party / `FM` | `对账单!B3` | Preserve the source name exactly; do not infer from a stamp alone when ambiguous. |
| Issuing contact / `ATTN` | `对账单!B4` | Leave blank and log when absent. |
| Issuing telephone | `对账单!B5` | Store as text. Leave blank and log when absent. |
| Issuing fax | `对账单!B6` | Store as text. Leave blank and log when absent. |
| Statement date | `对账单!B7` | Use a typed date only when the complete date is clear. Leave blank and log when absent. |
| Receiving party / `TO` | `对账单!H3` | Write only when clearly identified. |
| Receiving contact / `ATTN` | `对账单!H4` | Leave blank and log when absent. |
| Receiving telephone | `对账单!H5` | Store as text. Leave blank and log when absent. |
| Receiving fax | `对账单!H6` | Store as text. Leave blank and log when absent. |
| Receiving address | `对账单!H7` | Leave blank and log when absent. |
| Statement title/period | `对账单!A8` | Replace the generic title only when the source title or period is explicit. |

## Line-item mapping

Rows `10:29` are the writable detail rows.

| Source meaning | Column | Data rule |
| --- | --- | --- |
| Sequence | `A` | Keep the existing sequence formula/value unless row count changes with user approval. |
| Contract number | `B` | Text; preserve leading zeros and punctuation. |
| Order number | `C` | Text; preserve leading zeros and punctuation. |
| Material number | `D` | Text; preserve leading zeros, suffixes, and case. |
| Item name | `E` | Text exactly as confirmed by the three OCR passes. |
| Delivery note number | `F` | Text; do not coerce to a number. |
| Delivery date/time | `G` | Typed date when complete; otherwise text only after user confirmation. |
| Delivered quantity | `H` | Numeric value; do not substitute ordered quantity. |
| Unit | `I` | Text such as `KG`, normalized only under the permitted OCR rules. |
| Unit price | `J` | Numeric RMB value. Handwritten prices may be written only after three-pass consensus confirms both the value and the rows covered by any brace or annotation. Log that the price was handwritten. |
| Amount | `K` | Preserve the template formula `delivered quantity × unit price`; do not hardcode a recognized amount unless the user explicitly requires source-printed values. |
| Settlement method | `L` | Write only when shown on the source or supplied by the user. When absent, leave blank and log the missing field. |

The template total is in `对账单!K33` and sums `K10:K29`. The uppercase RMB total area is intentionally blank because automatic Chinese uppercase conversion is not defined by the template.

## Fields recorded in Markdown by default

The bundled template has no dedicated columns for these fields, so record them in the task Markdown log instead of dropping them:

- ordered quantity;
- packaging or remarks such as `25KG×8桶`;
- handwritten braces, correction marks, stamps, signatures, and margin annotations;
- source fields that cannot be mapped without changing their business meaning;
- missing `ATTN`, `TEL`, `FAX`, `DATE`, address, settlement method, or other expected metadata.

If a user-supplied workbook has an unambiguous destination for one of these fields, follow that workbook rather than this bundled-template rule.

## Verification

- Confirm formulas remain present in `K10:K29` and `K33`.
- Compare each document quantity subtotal and the grand total with the recognized source.
- Scan for formula errors and render `对账单!A1:L38` before delivery.
- Log any blank source field, manual price, unmapped field, layout change approved by the user, and verification limitation.
