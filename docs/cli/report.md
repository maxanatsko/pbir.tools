# Report Operations

Operations that affect the entire report: renaming, connecting to a different data model, converting between formats (PBIR/PBIP/PBIX), and combining or splitting reports.

## rename

```bash
pbir report rename "Report.Report" "New Report Name"
```

Updates the folder name and `.platform` displayName.

## rebind

Rebind a report to a different semantic model.

```bash
pbir report rebind "Report.Report" "Workspace.Workspace/Model.SemanticModel"
```

## convert

Convert report between formats: `pbir`, `pbip`, `pbix`.

```bash
pbir report convert "Report.Report" -F pbix                # PBIR -> PBIX
pbir report convert "Report.Report" -F pbip                # PBIR -> PBIP
pbir report convert "Report.pbip" -F pbir                  # PBIP -> PBIR
```

Conversion uses atomic staging: copies to a temp directory, then swaps into place. If the copy fails, the original target remains intact.

## merge

Combine multiple reports into one.

```bash
pbir report merge "R1.Report" "R2.Report" -o "Combined.Report"
```

## merge-to-thick

Merge a thin report with a semantic model into a thick PBIP.

```bash
pbir report merge-to-thick "Report.Report" "Model.SemanticModel"
```

## split-pages

Split a multi-page report into separate single-page reports.

```bash
pbir report split-pages "Report.Report" -o ./split
```

## split-from-thick

Split a thick PBIP into a thin report (publishes the model, rebinds the report).

```bash
pbir report split-from-thick ThickReport --target "Workspace/Model"
```

## clear-diagram

Delete the `semanticModelDiagramLayout.json` file.

```bash
pbir report clear-diagram "Report.Report"
```
