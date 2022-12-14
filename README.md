# Module 2 | Assignment - Wall Street

Explore green energy stock performance by analyzing financial data using VBA.

## Purpose of this analysis
This VBA helps to subtotal daily trading volume of each stock and also provided yearly yield based on the closed price on the first trading day and the last trading day over the year when the user requests.

On the other hand, the VBA also gives a timing it consumes for the macro.

## Results: Using images and examples of your code, compare the stock performance between 2018 and 2018, as well as the execution times of the original script and the refactored script.
Once the returns are positive, the cells would show in green and when the returns are negative, the cells would show in red. Per the result compared between 2017 returns and 2018 returns, it easily concludes that most stocks performs much better in 2017 than in 2018.[VBA_Challenge](/VBA_Challenge.xlsm)

Based on the timing below. My PC is very good. :)

![VBA_Challenge_2017](/VBA_Challenge_2017.png)
![VBA_Challenge_2018](/VBA_Challenge_2018.png)

## Summary

- What are the advantages of refactoring code?
  - It's easily readable and understandable with comments. 

- What are the disadvantages of refactoring code?
  - The original dataset has been filtered and sorted, which saves time to write scripts. However, as a beginner, I don't have enough examples to practice with raw data.

- How do these pros and cons apply to refactoring the original VBA script?
  - I got a great start with the refactoring the original VBA scripts. In the meantime, I should push myself to spend more time on more researching and apply different ideas with real scenarios.

[Challenge Solution](Challenge_Solution): The solution files for the module's challenge assignment are located in this folder.


## Challenge Solution

Sub AllStocksAnalysisRefactored()
    
    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    
    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    'Initialize array of all tickers
    Dim tickers(12) As String
    
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
    
    'Activate data worksheet
    Worksheets(yearValue).Activate
    
    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
    '1a) Create a ticker Index
    
    tickerIndex = 0

    
    '1b) Create three output arrays
    
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single
    
    
    ''2a) Create a for loop to initialize the tickerVolumes to zero.
  
    For i = 0 To 11
   
        tickerVolumes(i) = 0
      
    Next i
      
    ''2b) Loop over all the rows in the spreadsheet.
    
    For i = 2 To RowCount
    
        '3a) Increase volume for current ticker
            
        ticker = tickers(tickerIndex)
        
        tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value

        
        '3b) Check if the current row is the first row with the selected tickerIndex.
        'If  Then
            
        If Cells(i - 1, 1).Value <> ticker And Cells(i, 1).Value = ticker Then

            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value

        End If
        
            
        'End If
        
        '3c) check if the current row is the last row with the selected ticker
         'If the next row???s ticker doesn???t match, increase the tickerIndex.
        'If  Then
            
        If Cells(i + 1, 1).Value <> ticker And Cells(i, 1).Value = ticker Then

            tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
        
        
            '3d Increase the tickerIndex.
            
            tickerIndex = tickerIndex + 1
            
        End If
    
    Next i
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    
    For i = 0 To 11
        
        Worksheets("All Stocks Analysis").Activate

        Cells(4 + i, 1).Value = tickers(i)

        Cells(4 + i, 2).Value = tickerVolumes(i)

        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
        
        
    Next i
    
    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"
    Columns("B").AutoFit

    dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd
        
        If Cells(i, 3) > 0 Then
            
            Cells(i, 3).Interior.Color = vbGreen
            
        Else
        
            Cells(i, 3).Interior.Color = vbRed
            
        End If
        
    Next i
 
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub

Sub clearsheet()
 
 Cells.Clear
End Sub


## Module Solution

[Module Solution](Module_Solution): Any module demos or solution files for the module async content are located in this folder.

[VBA_Module_Challenge](/VBA_Module_Challenge.xlsm)

![VBA_Module_2017](/VBA_Module_2017.png)
![VBA_Module_2018](/VBA_Module_2018.png)

Sub MacroCheck()

 Dim testMessage As String
 
 testMessage = "Hello World!"

  MsgBox (testMessage)

End Sub

Sub DQAnalysis()

Worksheets("DQ Analysis").Activate

    Range("A1").Value = "DAQO (Ticker: DQ)"

    'Create a header row
    
    Cells(3, 1).Value = "Year"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"


    Worksheets("2018").Activate
    
     Dim startingPrice As Double
     Dim endingPrice As Double
     

    rowStart = 2

    rowEnd = Cells(Rows.Count, "A").End(xlUp).Row
'DELETE: rowEnd = 3013
'rowEnd code taken from https://stackoverflow.com/questions/18088729/row-count-where-data-exists
    totalVolume = 0
    
'loop over all the rows

    For i = rowStart To rowEnd
     
     'increase totalVolume
     

     If Cells(i, 1).Value = "DQ" And Cells(i - 1, 1).Value <> "DQ" Then
     
        'set starting price
     
     startingPrice = Cells(i, 6).Value
     
        totalVolume = totalVolume + Cells(i, 8).Value

    End If

    If Cells(i - 1, 1).Value <> "DQ" And Cells(i, 1).Value = "DQ" Then

            startingPrice = Cells(i, 6).Value

    End If

    If Cells(i + 1, 1).Value <> "DQ" And Cells(i, 1).Value = "DQ" Then

            endingPrice = Cells(i, 6).Value

    End If

    Next i

    Worksheets("DQ Analysis").Activate
    
    Cells(4, 1).Value = 2018
    
    Cells(4, 2).Value = totalVolume
    
    Cells(4, 3).Value = endingPrice / startingPrice - 1
    

End Sub


Sub AllStocksAnalysis()

   '1) Format the output sheet on All Stocks Analysis worksheet

    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    Range("A1").Value = "All Stocks (" + yearValue + ")"

  'Create a header row
  
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

 '2) Initialize array of all tickers
 
 Dim tickers(12) As String
 
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
 
  '3a) Initialize variables for starting price and ending price
  
  Dim startingPrice As Single
  Dim endingPrice As Single
  
    '3b) Activate data worksheet
    
    Worksheets(yearValue).Activate
    
     '3c) Get the number of rows to loop over
     
     RowCount = Cells(Rows.Count, "A").End(xlUp).Row
     
     
      '4) Loop through tickers
      
   For i = 0 To 11
   
       ticker = tickers(i)
       totalVolume = 0

    '5) loop through rows in the data
    
   Worksheets(yearValue).Activate
    
       For j = 2 To RowCount
    
           '5a) Get total volume for current ticker
           
           If Cells(j, 1).Value = ticker Then

                totalVolume = totalVolume + Cells(j, 8).Value

            End If
           
           '5b) get starting price for current ticker
           
            If Cells(j - 1, 1).Value <> ticker And Cells(j, 1).Value = ticker Then

                startingPrice = Cells(j, 6).Value

            End If


           '5c) get ending price for current ticker

            If Cells(j + 1, 1).Value <> ticker And Cells(j, 1).Value = ticker Then

                 endingPrice = Cells(j, 6).Value

            End If

       Next j
       
       '6) Output data for current ticker

Worksheets("All Stocks Analysis").Activate

Cells(4 + i, 1).Value = ticker

Cells(4 + i, 2).Value = totalVolume

Cells(4 + i, 3).Value = endingPrice / startingPrice - 1
   
   Next i

    'Formatting
    Worksheets("All Stocks Analysis").Activate
    
    Range("A3:C3").Font.FontStyle = "Bold"
    
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    
    Range("B4:B15").NumberFormat = "#,##0"
    
    Range("C4:C15").NumberFormat = "0.00%"
    
    Columns("B").AutoFit


'condition format

Worksheets("All Stocks Analysis").Activate

    dataRowStart = 4
    dataRowEnd = 15
    
    For i = dataRowStart To dataRowEnd

        If Cells(i, 3) > 0 Then

            'Color the cell green
            Cells(i, 3).Interior.Color = vbGreen

        ElseIf Cells(i, 3) < 0 Then

            'Color the cell red
            Cells(i, 3).Interior.Color = vbRed

        Else

            'Clear the cell color
            Cells(i, 3).Interior.Color = xlNone

        End If


    
    'Dim startTime As Single
    'Dim endTime  As Single

    'yearValue = InputBox("What year would you like to run the analysis on?")

 'Sheets(yearValue).Activate
 
'Range("A1").Value = "All Stocks (" + yearValue + ")"

       'startTime = Timer
       
Next i

    endTime = Timer
    
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub

Sub ClearWorksheet()

    Cells.Clear
    
End Sub

