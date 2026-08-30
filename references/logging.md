# Markdown audit logs

## Image-to-Excel task log

Create a task log beside the output workbook. Use a descriptive name such as `<供应商>-<期间>-单据识别写入日志.md`.

Include:

- task date and source filenames;
- target workbook and output workbook filenames;
- template source and version: user-supplied workbook or bundled generic template;
- three-pass consensus status for every image;
- each mismatch or unreadable field with all three pass values;
- user confirmations, including the exact confirmed value and confirmation time/order;
- the field-to-cell or field-to-column mapping used;
- source fields that had no Excel destination;
- Excel fields left blank because the source lacked data;
- handwritten notes, braces, stamps, signatures, or annotations not written to Excel;
- formulas and totals created or preserved;
- verification results, formula-error scan, and visual review status;
- unresolved questions and remaining risks.

Never fabricate unreadable signatures or annotations. Record that the content exists and is unresolved.

## Logging pauses

When a recognition or mapping issue triggers a pause, write the issue to the log before asking the user. Do not mark it resolved until the user supplies or approves a value. On resumption, append the decision instead of deleting the original discrepancy record.

## Reconciliation report

Use the structure in [pdf-reconciliation.md](pdf-reconciliation.md). Keep confirmed mismatches separate from information gaps. A report with no confirmed or unresolved comparable differences must include the date, document number, supplier, period, and the explicit conclusion `对账无误`.
