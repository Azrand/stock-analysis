# Green Stock Analysis

## Overview of Project
Utilizing the Green Stock data from 2017 to 2018, we were tasked with a project from the University of Wisconsin. In this scenario for the project, our client Steven asked that we refactor the code that we created in VBA so that it can show the returns of all green stocks on a new Excel sheet for the given years. Steven was concerned about the efficiency of our current code and hoped that our refactored code would be quicker due to the thousands of rows it has to go through. On the new sheet, Steven needed it to display the year of the stocks, ticker symbol, the total daily volume, and the end-of-year return for each stock. 


## Results
Below is the VBA code that needed to be refactored. 
```
Sub AllStocksAnalysToBeRefactored()
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
    

    '1b) Create three output arrays
    
    
    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    
        
    ''2b) Loop over all the rows in the spreadsheet.
    For i = 2 To RowCount
    
        '3a) Increase volume for current ticker
        
        
        '3b) Check if the current row is the first row with the selected tickerIndex.
        'If  Then
            
            
            
        'End If
        
        '3c) check if the current row is the last row with the selected ticker
         'If the next row's ticker doesn't match, increase the tickerIndex.
        'If  Then
            
            

            '3d Increase the tickerIndex.
            
            
        'End If
    
    Next i
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11
        
        Worksheets("All Stocks Analysis").Activate
        
        
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
```
The code was able to produce the following tables. 

![2017 Stock](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2017.png)
![2018 Stock](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2018.png)

Below is the VBA code after being refactored.
```
Sub AllStocksAnalysisRefactored()
    
    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    'Title Analysis
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
    
    'Count the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
    '1a) Create a ticker index to reference proper ticker in the arrays.
    Dim tickerIndex As Integer
    'Initiate tickerIndex at zero.
    tickerIndex = 0
    

    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single
    
    
    
    '2a) Create for loop to analyze each ticker in the array.
    For tickerIndex = 0 To 11
    'Initiate each ticker's volume at zero.
    tickerVolumes(tickerIndex) = 0
    
    'Activate data worksheet
    Worksheets(yearValue).Activate
        
        '2b) Loop over all the rows in the spreadsheet.
        For i = 2 To RowCount
        
            '3a) Increase volume for current ticker.
            tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
    
        
            '3b) Check if the current row is the first row with the current ticker.
                    
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
                'if it is the first row for current ticker, set starting price.
                tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
            
            'End If
            End If
            
            
        '3c) Check if the current row is the last row with the current ticker.

            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                'if it is the last row for current ticker, set ending price.
                tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
            
            'End if
            End If
            
        '3d) Check if the current row is the last row with the current ticker.
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                
                'if it is, increase tickerIndex to move on to next ticker in array.
                tickerIndex = tickerIndex + 1
            
            'End If
            End If
    
        Next i
        
    Next tickerIndex
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    
    For i = 0 To 11
        
        'Activate Output Worksheet
        Worksheets("All Stocks Analysis").Activate
        
        'Ticker Row Label
        Cells(4 + i, 1).Value = tickers(i)
        
        'Sum of Volume
        Cells(4 + i, 2).Value = tickerVolumes(i)
        
        'ReturnValue
        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
        

    
    Next i
End Sub
```

Below is the code used within the module
```
Sub AllStocksAnalysis()

    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")
    
    startTime = Timer

'1) Format the output sheet on All Stocks Analysis Worksheet

    'Activate "All Stocks Analysis" worksheet
    Worksheets("All Stocks Analysis").Activate

    'Title Analysis
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    
    'Create a Header Row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"
    
'2)Initialize an array of all tickers.
    
    'Declare an array with 12 string elements
    Dim tickers(12) As String
    
        'Assign tickers to an element in the array
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
        
'3) Prepare for the analysis of all tickers.

    '3a) Initialize variables for the starting price and ending price.
    
        'Creating a Variable for Starting & Ending Price
        Dim startingPrice As Double
        Dim endingPrice As Double
    
    '3b) Activate the data worksheet.
        
        Worksheets(yearValue).Activate
        
    '3c) Find the number of rows to loop over.
        
        rowStart = 2
        'DELETE: rowEnd = 3013
        'rowCount code taken from https://stackoverflow.com/questions/18088729/row-count-where-data-exists
        RowCount = Cells(Rows.Count, "A").End(xlUp).Row
        
'4) Loop through the tickers.
    
    For i = 0 To 11
    ticker = tickers(i)
    totalVolume = 0

'5) Loop through the rows in the data.

        'Activate Data Worksheet
        Worksheets(yearValue).Activate
        
        For j = rowStart To RowCount
        

    
    '5a) Find the total volume for the current ticker.
    
            'Identify ticker
            If Cells(j, 1).Value = ticker Then
                
                'increase ticker totalVolume by the value in the current row
                totalVolume = totalVolume + Cells(j, 8).Value
            
            End If
            
    '5b) Find the starting price for the current ticker.
    
            'Identify first row of ticker
            If Cells(j, 1).Value = ticker And Cells(j - 1, 1).Value <> ticker Then
                'set starting price
                startingPrice = Cells(j, 6).Value
                
            End If
            
    '5c) find the ending price for the current ticker.
    
            'Identify last row of ticker
            If Cells(j, 1).Value = ticker And Cells(j + 1, 1).Value <> ticker Then
                'set ending price
                endingPrice = Cells(j, 6).Value
                
            End If
            
        Next j
        
    
    
'6) Output the data for the current ticker.

        'Activate Output Worksheet
        Worksheets("All Stocks Analysis").Activate
        
        'Ticker header
        Cells(i + 4, 1).Value = ticker
    
        'Sum of Volume
        Cells(i + 4, 2).Value = totalVolume
    
        'Return Value
        Cells(i + 4, 3).Value = endingPrice / startingPrice - 1
        
    Next i
    
        endTime = Timer
        MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

   

End Sub
```
**Below is the 2017 run time for the VBA code**

My Code Run Time

![VBA_Challenge_2017_MyCode.png](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2017_MyCode.png)

Before Refactoring

![VBA_Challenge_2017_BeforeRefractored.png](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2017_BeforeRefractored.png)

After Refactoring

![VBA_Challenge_2017_Refractored.png](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2017_Refractored.png)


**Below is the 2018 run time for the VBA code**

My Code Run Time

![VBA_Challenge_2018_MyCode.png](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2018_MyCode.png)

Before Refactoring

![VBA_Challenge_2018_BeforeRefractored.png](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2018_BeforeRefractored.png)

After Refactoring

![VBA_Challenge_2018_Refractored.png](https://github.com/Azrand/stock-analysis/blob/main/resources/VBA_Challenge_2018_Refractored.png)

## Summary
### Advantages and Disadvantages of Refactoring
The advantages of refactoring code is that it allows a more readable and managable experience for team members. The disadvantage is introducing bugs, errors, or breaking the code. 


I broke the code several times during the project. I had to start over quite a bit. I utilized GitHub, StackOverflow and Google to help me out, but in the end I utilized the code within the module to increase the speed and performance of the refactored code. There's only a neglible decrease in time, but I'm not sure if that's due to machine performance or because of code.
