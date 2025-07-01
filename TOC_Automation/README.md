# Hyper Link Data - Excel VBA Macro

This Excel VBA macro automatically creates a Table of Contents (TOC) with clickable hyperlinks to other worksheets in the workbook.

---

## Features

- Prompts the user to select a starting cell where the TOC will be inserted.
- Lists all worksheets in the workbook except the active one.
- Creates hyperlinks to each worksheet's cell A1.
- Displays the worksheet name and the value of cell A1 from the respective sheet.
- Automatically inserts the TOC vertically starting from the selected cell.

---



## How to Use

1. Open the Excel workbook where you want to add the TOC.
2. Press `ALT + F11` to open the VBA Editor.
3. Insert a new module (`Insert` > `Module`) and paste the macro code below.
4. Run the macro `auto_Toc_creation` (e.g., from the VBA editor or assign it to a button).
5. When prompted, select the cell where you want the Table of Contents to start.
6. The macro will insert hyperlinks and display the first cell's value from each sheet in the active worksheet.

---

## VBA Code

```vba
Sub auto_Toc_creation()
    Dim startcell As Range
    Dim sh As Worksheet
    Dim shName As String
    
    ' Prompt user to select the starting cell for TOC
    Set startcell = Excel.Application.InputBox("Where do you want to insert the TOC?" & vbNewLine & "Please select a cell:", "Insert TOC", , , , , , 8)
    
    Set startcell = startcell.Cells(1, 1)
    
    ' Loop through all worksheets except the active one
    For Each sh In Worksheets
        If ActiveSheet.Name <> sh.Name Then
            shName = sh.Name
            
            ' Add hyperlink to the sheet's cell A1
            ActiveSheet.Hyperlinks.Add Anchor:=startcell, Address:="", SubAddress:=shName & "!A1", TextToDisplay:=shName
            
            ' Display the value of cell A1 from the target sheet next to hyperlink
            startcell.Offset(0, 1).Value = sh.Range("A1").Value
            
            ' Move down one cell for the next hyperlink
            Set startcell = startcell.Offset(1, 0)
        End If
    Next sh
End Sub
