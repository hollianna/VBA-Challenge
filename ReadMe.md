Non-recursive and Recursive Stock Analysis

This project utilizes macros to analyze and highlight stock changes. It combines the data for each ticker symbol, inserts columns to clearly highlight increases and decreases, and finally summarizes the greatest increase, decrease, and total volume. 

The code in this project was developed in class through edX Boot Camps LLC with University of Pennsylvania as a group project.
 

Set dimensions:

    ' Set dimensions
    Dim total As Double
    Dim i As Long
    Dim change As Double
    Dim j As Integer
    Dim start As Long
    Dim rowCount As Long
    Dim percentChange As Double
    
    'Dim days As Integer
    Dim dailyChange As Double
    Dim averageChange As Double

Loop through the stocks for each quarter and create columns for Ticker Symbol, Quarterly Change, Percent Change, Total Stock Volume, Ticker, Value, Greatest Increase, Greatest Decrease, and Greatest Total Volume:

  ' Set title row
    Range("I1").Value = "Ticker"
    Range("J1").Value = "Quarterly Change"
    Range("K1").Value = "Percent Change"
    Range("L1").Value = "Total Stock Volume"
    Range("P1").Value = "Ticker"
    Range("Q1").Value = "Value"
    Range("O2").Value = "Greatest % Increase"
    Range("O3").Value = "Greatest % Decrease"
    Range("O4").Value = "Greatest Total Volume"

Set the following initial values:

   ' Set initial values
    j = 0
    total = 0
    change = 0
    start = 2

Get the row number of the last row with data:
    ' get the row number of the last row with data
    rowCount = Cells(Rows.Count, "A").End(xlUp).Row

Print total for each ticker symbol and calculate percent change:

For i = 2 To rowCount

        ' If ticker changes then print results
        If Cells(i + 1, 1).Value <> Cells(i, 1).Value Then

            ' Stores results in variables
            total = total + Cells(i, 7).Value

            ' Handle zero total volume
            If total = 0 Then
                ' print the results
                Range("I" & 2 + j).Value = Cells(i, 1).Value
                Range("J" & 2 + j).Value = 0
                Range("K" & 2 + j).Value = "%" & 0
                Range("L" & 2 + j).Value = 0

            Else
                ' Find First non zero starting value
                If Cells(start, 3) = 0 Then
                    For find_value = start To i
                        If Cells(find_value, 3).Value <> 0 Then
                            start = find_value
                            Exit For
                        End If
                     Next find_value
                End If

                ' Calculate Change
                change = (Cells(i, 6) - Cells(start, 3))
                percentChange = change / Cells(start, 3)

                ' start of the next stock ticker
                start = i + 1

                ' print the results
                Range("I" & 2 + j).Value = Cells(i, 1).Value
                Range("J" & 2 + j).Value = change
                Range("J" & 2 + j).NumberFormat = "0.00"
                Range("K" & 2 + j).Value = percentChange
                Range("K" & 2 + j).NumberFormat = "0.00%"
                Range("L" & 2 + j).Value = total

Change color of output so positives are green and negatives are red:

                ' colors positives green and negatives red
                Select Case change
                    Case Is > 0
                        Range("J" & 2 + j).Interior.ColorIndex = 4
                    Case Is < 0
                        Range("J" & 2 + j).Interior.ColorIndex = 3
                    Case Else
                        Range("J" & 2 + j).Interior.ColorIndex = 0
                End Select

            End If

            ' reset variables for new stock ticker
            total = 0
            change = 0
            j = j + 1
            'days = 0

        ' If ticker is still the same add results
        Else
            total = total + Cells(i, 7).Value

        End If

    Next i

Determine max and min percentage:

    ' take the max and min and place them in a separate part in the worksheet
    Range("Q2") = "%" & WorksheetFunction.Max(Range("K2:K" & rowCount)) * 100
    Range("Q3") = "%" & WorksheetFunction.Min(Range("K2:K" & rowCount)) * 100
    Range("Q4") = WorksheetFunction.Max(Range("L2:L" & rowCount))

    ' returns one less because header row not a factor
    increase_number = WorksheetFunction.Match(WorksheetFunction.Max(Range("K2:K" & rowCount)), Range("K2:K" & rowCount), 0)
    decrease_number = WorksheetFunction.Match(WorksheetFunction.Min(Range("K2:K" & rowCount)), Range("K2:K" & rowCount), 0)
    volume_number = WorksheetFunction.Match(WorksheetFunction.Max(Range("L2:L" & rowCount)), Range("L2:L" & rowCount), 0)

    ' final ticker symbol for total, greatest % of increase and decrease, and average
    Range("P2") = Cells(increase_number + 1, 9)
    Range("P3") = Cells(decrease_number + 1, 9)
    Range("P4") = Cells(volume_number + 1, 9)

End Sub

To loop through all worksheets in the workbook, add the following at the beginning of the script:

    ' Loop through each worksheet in the workbook
    Dim ws As Worksheet
    For Each ws In ThisWorkbook.Worksheets
        ws.Activate  ' Activate the worksheet



        
 <img width="1031" alt="Screenshot 2024-10-10 at 6 11 52 AM" src="https://github.com/user-attachments/assets/f748c4e6-e42a-40d2-8907-af6de822c67c">
 
<img width="1029" alt="Screenshot 2024-10-10 at 6 13 26 AM" src="https://github.com/user-attachments/assets/cd2d1129-f9e1-4ddd-8549-1b59689f92c6">

<img width="1029" alt="Screenshot 2024-10-10 at 6 13 39 AM" src="https://github.com/user-attachments/assets/aac67af4-ab5b-46e6-82b1-aed77019c888">

<img width="1035" alt="Screenshot 2024-10-10 at 6 13 52 AM" src="https://github.com/user-attachments/assets/127c458a-17cc-46f0-beed-0b7a67829165">




