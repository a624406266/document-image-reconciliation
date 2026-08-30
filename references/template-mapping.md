# Bundled Dongguan Chaobang statement template

## Source and scope

- Asset: `assets/供应商名称+x月份对账单-东莞超邦OK（模板）.xlsx`
- Template version: `2.0`
- Worksheet: `东莞超邦`
- Used range: `A1:L38`
- Intended recipient: `东莞市超邦电子实业有限公司`
- Detail capacity: rows `10:30`; rows `10:29` already contain sequence numbers `1:20`, and row `30` is an unnumbered overflow row.

This asset is the user-provided workbook and is the authoritative layout. Preserve its sheet name, merges, row and column sizes, borders, fonts, number formats, recipient information, notes, signature areas, and formulas. Do not redesign or restyle it.

Copy the asset to the task output directory before writing. Never edit the bundled asset in place.

## Required metadata gate

The output cannot be completed until all of these supplier-side values are authoritative:

- supplier name;
- supplier address;
- `FM`;
- supplier `ATTN`;
- supplier `TEL`;
- supplier `FAX`;
- statement `DATE`;
- reconciliation month.

Use three-pass OCR values only when all three passes agree. If any item is missing, illegible, ambiguous, or inconsistent, stop before Excel writing and ask the user for all missing items. Do not merely leave these fields blank and record them in Markdown.

If the user confirms `无` or `不适用`, ask whether to write `无` or keep the cell blank, then record the decision in the task log.

The recipient-side values already present in the template (`TO`, recipient `ATTN`, recipient `TEL`, and recipient address) are fixed template data. Preserve them unless the source conflicts or the user asks to change them. If a fixed value conflicts with the images, pause and ask the user.

## Metadata and title mapping

| Meaning | Destination | Rule |
| --- | --- | --- |
| Supplier name / workbook title | `东莞超邦!A1` | Replace `供应商名称` with the confirmed full supplier name. |
| Supplier address | `东莞超邦!A2` | Replace `地址` with the confirmed supplier address. |
| `FM` | `东莞超邦!B3` | Required; preserve the confirmed text exactly. |
| Supplier `ATTN` | `东莞超邦!B4` | Required; preserve names as text. |
| Supplier `TEL` | `东莞超邦!B5` | Required; store as text to preserve punctuation and leading zeros. |
| Supplier `FAX` | `东莞超邦!B6` | Required; store as text. |
| Statement `DATE` | `东莞超邦!B7` | Required; use a typed date only when the complete date is confirmed. |
| Reconciliation month/title | `东莞超邦!A8` | Replace `X月份对账单（含税）` with `<确认月份>月份对账单（含税）`. |
| Supplier signature label | `东莞超邦!A38` | Replace the example supplier name with `<确认供应商名称>：`. |

Keep the existing recipient-side data at `I3:I7` unless the user confirms a change. Do not overwrite it with supplier data.

## Output filename

The final workbook filename must be:

`<供应商名称>+<月份>月份对账单-东莞超邦OK.xlsx`

Example: `广东省益凯隆新型材料科技有限公司+7月份对账单-东莞超邦OK.xlsx`

Replace filesystem-reserved characters in the supplier name only when necessary, and record the filename normalization in the log. Do not deliver a file whose name still contains `供应商名称`, `x月份`, or `模板`.

## Line-item mapping

| Source meaning | Column | Data rule |
| --- | --- | --- |
| Sequence | `A` | Preserve rows `10:29` as `1:20`. Ask before using row `30` or adding rows. |
| Contract number | `B` | Text; preserve leading zeros and punctuation. |
| Order number | `C` | Text; preserve leading zeros and punctuation. |
| Material number | `D` | Text; preserve leading zeros, suffixes, and case. |
| Item name | `E` | Text exactly as confirmed by the three OCR passes. |
| Delivery note number | `F` | Text; do not coerce to a number. |
| Delivery date/time | `G` | Typed date when complete; otherwise ask the user. |
| Delivered quantity | `H` | Numeric value; do not substitute ordered quantity. |
| Unit | `I` | Text such as `KG`, normalized only under the permitted OCR rules. |
| Unit price | `J` | Numeric RMB value. A handwritten price may be written only after three-pass consensus confirms the value and the rows covered by its brace or annotation. Log that it was handwritten. |
| Amount | `K` | When the source does not print an amount and quantity plus unit price are confirmed, use the auditable row formula `=Hn*Jn`. If a printed amount differs from the calculation, pause and ask. |
| Settlement method | `L` | Write only when shown or supplied by the user. If absent, record it in the Markdown log unless the user requires a value. |

Preserve the template formulas `东莞超邦!I33 = SUM(K10:K30)` and `东莞超邦!I34 = I33`. Do not move them even though their displayed result spans merged cells.

## Markdown-only fields

The template has no dedicated columns for these fields, so record them in the task Markdown log:

- ordered quantity;
- packaging or remarks such as `25KG×8桶`;
- handwritten braces, correction marks, stamps, signatures, and margin annotations;
- source fields that cannot be mapped without changing their business meaning.

## Verification

- Confirm `A1`, `A2`, `B3:B7`, `A8`, and `A38` contain the resolved values and no placeholders remain.
- Confirm recipient data at `I3:I7` was preserved unless the user approved a change.
- Confirm the detail rows and amount formulas/values match the source.
- Confirm `I33` and `I34` retain their original formulas.
- Compare quantity and amount totals with the recognized source, scan for formula errors, and render `东莞超邦!A1:L38` before delivery.
