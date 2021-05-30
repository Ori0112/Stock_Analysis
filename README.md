# Stock_Analysis

## Overview

The purpose of this project was to helping Steve look into DAQO New Energy Corporation’s stock, for Steve’s parents first investment. Steve is concerned about diversifying their funds and wants to analyze a handful of green energy stock. 

Therefore, we are in charge of analyzing the data that Steve has collected. We will be collecting key stock information using EXcel VBA code to determine if it Steve's parents should invest in DAQO. In the very end we want to make the process as productive as possible and will modify our original code, to help Steve receive the data of the total daily volume and ROI efficiently.

## Background

Steve's parents were mainly interested in finding the total daily volume and yearly return of investments for DAQO, concerned with how actively DAQO was traded in 2018. Our method was to find these KPIs for Steve's parents, as well as, compare DAQO with 11 other green energy stock.  

Using VBA we looped through evey row in our stock data to find the ticker values, and determinte the total volumes and yearly returns for DAQO in 2018. Then we used the same code to run through all the the green energy stocks of 2017 and 2018. In the end, we modified our original code to pull the data we needed more efficiently.

## Results

The code that was developed was in charge of automating three output arrays, format a chart where we would be presenting our findings, using a program flow that loops through all the tickers, and evaluting the three KPIs.

    Sub AllStocksAnalysis()
    
    Dim startTime As Single
    Dim endTime  As Single
     
    '1) Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
   
    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer

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

        With Range("C4:C15")
                    .NumberFormat = "0.0%"
                    .Value = .Value
        End With

    Next i
 
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)
    
 
