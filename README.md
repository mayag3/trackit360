# trackit360




## 🧾 Overview

**It is a two-part inventory management system designed to streamline physical stock tracking and comparison with system records. Built initially using Excel macros, the goal is to transition into a web app using barcode scanning to manage part numbers, quantities, and storage locations efficiently.

---

## 🔧 Functionality

### ✅ Part 1: Physical Stock Count (Barcode-Based Entry)

- A **barcode scanner** is used to input product data: `Part Number`, `Quantity`, and `Location`.
- ![image](https://github.com/user-attachments/assets/fd73a4ea-f652-48a9-aa0a-27a4462b865a)

- User enters these values via a UI (Sheet 1). Upon submission:
  - Values are copied to cells `F7`, `F8`, and `F9`.
  - ![image](https://github.com/user-attachments/assets/2a56d7f9-a1e8-4593-9689-12154b75882b)

  - These are **transposed** and sent to `Sheet 3` for historical records.
  - Simultaneously, data is inserted into `Sheet 2` with row-shift to maintain chronological logs.
  - After submission, `Part Number` and `Quantity` are cleared, but **Location persists** unless manually changed.
  ---

## 💻 Excel Macro (VBA Snippet)

The following VBA macro handles the physical data capture and record insertion in Excel:

```vba
Sub Data()
'
' Data Macro
'
    Range("F17:F19").Select
    Selection.Copy
    Sheets("Sheet3").Select
    Range("A1").Select
    Selection.PasteSpecial Paste:=xlPasteValues, Operation:=xlNone, SkipBlanks _
        :=False, Transpose:=True
    Range("A1:C1").Select
    Application.CutCopyMode = False
    Selection.Copy
    Sheets("Sheet2").Select
    Range("B3").Select
    Selection.Insert Shift:=xlDown
    Sheets("Sheet1").Select
    Range("F6").Select
    Application.CutCopyMode = False
    Selection.ClearContents
    Range("F8").Select
    Selection.ClearContents
End Sub

```


This ensures rapid and clean input of physical stock data via scanning.

---


### 🔍 Part 2: System Data Comparison

- A separate **System Data Sheet** contains default information for all parts: `Part Number`, `Default Quantity`, and `Default Location`.
- After accumulating physical scan data:
  1. **Sum quantities** for each distinct `Part Number`.
  2. Compare with system quantities and locations.
  3. If there are **multiple or mismatched locations**, they are **flagged**.
  4. Final output is a mismatch report showing discrepancies in both **quantity** and **location**.

This allows for audits between recorded stock and physical presence.

---
