# Analytics Engineering Sample DAX Measures

This file contains sample DAX measures from the Analytics Engineering semantic model.

## Avg Sales Amount per Item

```DAX
Avg Sales Amount per Item =
AVERAGEX(
    ADDCOLUMNS(
        SUMMARIZECOLUMNS(
            dimproduct[ItemID],
            dimproduct[ItemName],
            "Sales", [Sales Amount],
            "Qty", [Quantity]
        ),
        "Avg Price", DIVIDE([Sales], [Qty])
    ),
    [Avg Price]
)
```

## Customers with 100 + Orders

```DAX
Customers with 100 + Orders =
COUNTROWS(
    FILTER(
        SUMMARIZECOLUMNS(
            dimcustomer[CustomerID],
            "OrderCount", COUNTROWS(factorders)
        ),
        [OrderCount] >= 100
    )
)
```

## First Order Date

```DAX
First Order Date =
MINX(
    RELATEDTABLE(factorders),
    factorders[OrderDate]
)
```

## Avg Revenue per Day

```DAX
Avg Revenue per Day =
DIVIDE(
    [Revenue Amount],
    DISTINCTCOUNT(factorders[OrderDate])
)
```

## Avg Unit Price

```DAX
Avg Unit Price =
AVERAGE(factorders[_UnitPrice])
```

## BLANK_dimdate_Rows

```DAX
BLANK_dimdate_Rows =
COUNTROWS(
    FILTER(
        dimdate,
        ISBLANK(dimdate[Year])
    )
)
```

## Customer Sales Rank

```DAX
Customer Sales Rank =
RANKX(
    ALL(dimcustomer[CustomerName]),
    [Sales Amount],
    ,
    DESC
)
```

## Customers with Orders

```DAX
Customers with Orders =
COUNTROWS(
    DISTINCT(factorders[CustomerID])
)
```

## Customers With Orders (Orphan-Safe)

```DAX
Customers With Orders (Orphan-Safe) =
COUNTROWS(
    DISTINCT(factorders[CustomerID])
)
```

## Date With Highest Sales

```DAX
Date With Highest Sales =
MAXX(
    TOPN(
        1,
        VALUES(dimdate[OrderDate]),
        [Sales Amount], DESC,
        dimdate[OrderDate], DESC
    ),
    dimdate[OrderDate]
)
```

## DistinctDaysfactorders

Calculates the number of unique order dates in the `factorders` table.

```DAX
DistinctDaysfactorders =
DISTINCTCOUNT(factorders[OrderDate])
```

## Has Orders

```DAX
Has Orders =
IF(
    COUNTROWS(RELATEDTABLE(factorders)) > 0,
    1,
    0
)
```

## Is Year In Scope

```DAX
Is Year In Scope =
IF(
    ISINSCOPE(dimdate[Year]),
    1,
    0
)
```

## Order Count

```DAX
Order Count =
COUNTROWS(factorders)
```

## Quantity

```DAX
Quantity =
SUM(factorders[_Quantity])
```

## Quantity with Iterator

```DAX
Quantity with Iterator =
SUMX(
    factorders,
    factorders[_Quantity]
)
```

## Revenue Amount

```DAX
Revenue Amount =
[Sales Amount] + [Tax Amount]
```

## Road-650 Red Sales Amount

```DAX
Road-650 Red Sales Amount =
CALCULATE(
    [Sales Amount],
    dimproduct[ItemName] = "Road-650 Red"
)
```

## Road-650 Red Sales Amount 2026

```DAX
Road-650 Red Sales Amount 2026 =
CALCULATE(
    [Sales Amount],
    dimproduct[ItemName] = "Road-650 Red",
    dimdate[Year] = 2026
)
```

## Road-650 Red Sales Amount using All

```DAX
Road-650 Red Sales Amount using All =
CALCULATE(
    [Sales Amount],
    FILTER(
        ALL(dimproduct[ItemName]),
        dimproduct[ItemName] = "Road-650 Red"
    )
)
```

## Sales Amount

```DAX
Sales Amount =
SUMX(
    factorders,
    factorders[_Quantity] * factorders[_UnitPrice]
)
```

## Sales Amount (Products Containing "Mountain")

```DAX
Sales Amount (Products Containing "Mountain") =
CALCULATE(
    [Sales Amount],
    KEEPFILTERS(
        FILTER(
            factorders,
            CONTAINSSTRING(
                RELATED(dimproduct[ItemName]),
                "Mountain"
            )
        )
    )
)
```

## Sales Amount (Var Version)

```DAX
Sales Amount (Var Version) =
SUMX(
    factorders,
    VAR Q = factorders[_Quantity]
    VAR P = factorders[_UnitPrice]
    RETURN
        Q * P
)
```

## Sales Amount % of All Customers

```DAX
Sales Amount % of All Customers =
DIVIDE(
    [Sales Amount],
    CALCULATE(
        [Sales Amount],
        ALL(dimcustomer)
    )
)
```

## Sales Amount % of All Products

```DAX
Sales Amount % of All Products =
DIVIDE(
    [Sales Amount],
    [Sales Amount Ignoring Product]
)
```

## Sales Amount % of All Years

```DAX
Sales Amount % of All Years =
DIVIDE(
    [Sales Amount],
    CALCULATE(
        [Sales Amount],
        ALL(dimdate[Year])
    )
)
```

## Sales Amount % of All Years with Variable

```DAX
Sales Amount % of All Years with Variable =
VAR SalesOfAllYears =
    CALCULATE(
        [Sales Amount],
        ALL(dimdate[Year])
    )
RETURN
    DIVIDE(
        [Sales Amount],
        SalesOfAllYears
    )
```

## Sales Amount % of Top 5 Products

```DAX
Sales Amount % of Top 5 Products =
DIVIDE(
    [Sales Amount],
    [Top 5 Products Sales Amount]
)
```

## Sales Amount % of Year

```DAX
Sales Amount % of Year =
DIVIDE(
    [Sales Amount],
    CALCULATE(
        [Sales Amount],
        ALLEXCEPT(
            dimdate,
            dimdate[Year]
        )
    )
)
```

## Sales Amount All Dates

```DAX
Sales Amount All Dates =
CALCULATE(
    [Sales Amount],
    ALL(dimdate)
)
```

## Sales Amount for Large Orders

```DAX
Sales Amount for Large Orders =
CALCULATE(
    [Sales Amount],
    FILTER(
        factorders,
        factorders[_Quantity] >= 10
    )
)
```

## Sales Amount Ignoring Product

```DAX
Sales Amount Ignoring Product =
CALCULATE(
    [Sales Amount],
    REMOVEFILTERS(dimproduct)
)
```

## Sales Amount Last Month

```DAX
Sales Amount Last Month =
CALCULATE(
    [Sales Amount],
    DATEADD(
        dimdate[OrderDate],
        -1,
        MONTH
    )
)
```

## Sales Amount Last Year

```DAX
Sales Amount Last Year =
CALCULATE(
    [Sales Amount],
    SAMEPERIODLASTYEAR(dimdate[OrderDate])
)
```

## Sales Amount of Large Orders

```DAX
Sales Amount of Large Orders =
SUMX(
    FILTER(
        factorders,
        factorders[_Quantity] >= 100
    ),
    factorders[_Quantity] * factorders[_UnitPrice]
)
```

## Sales Amount on Highest Date

```DAX
Sales Amount on Highest Date =
MAXX(
    VALUES(dimdate[OrderDate]),
    [Sales Amount]
)
```

## Sales Amount PY

```DAX
Sales Amount PY =
CALCULATE(
    [Sales Amount],
    SAMEPERIODLASTYEAR(dimdate[OrderDate])
)
```

## Sales Amount Rolling 30 Days

```DAX
Sales Amount Rolling 30 Days =
CALCULATE(
    [Sales Amount],
    DATESINPERIOD(
        dimdate[OrderDate],
        MAX(dimdate[OrderDate]),
        -30,
        DAY
    )
)
```

## Sales Amount Running

```DAX
Sales Amount Running =
CALCULATE(
    [Sales Amount],
    FILTER(
        ALL(dimdate[OrderDate]),
        dimdate[OrderDate] <= MAX(dimdate[OrderDate])
    )
)
```

## Sales Amount YoY %

```DAX
Sales Amount YoY % =
DIVIDE(
    [Sales Amount] - [Sales Amount Last Year],
    [Sales Amount Last Year]
)
```

## Sales Amount YoY % Variable

```DAX
Sales Amount YoY % Variable =
VAR SalesAmountLastYear =
    [Sales Amount Last Year]
RETURN
    DIVIDE(
        [Sales Amount] - SalesAmountLastYear,
        SalesAmountLastYear
    )
```

## Sales Amount YTD

```DAX
Sales Amount YTD =
TOTALYTD(
    [Sales Amount],
    dimdate[OrderDate]
)
```

## SelectedYear

```DAX
SelectedYear =
SELECTEDVALUE(dimdate[Year])
```

## Tax Amount

```DAX
Tax Amount =
SUM(factorders[_Tax])
```

## Top 3 Visible Products Sales

```DAX
Top 3 Visible Products Sales =
VAR CurrentProducts =
    VALUES(dimproduct[ItemID])
VAR Top3 =
    TOPN(
        3,
        CurrentProducts,
        [Sales Amount],
        DESC
    )
RETURN
    CALCULATE(
        [Sales Amount],
        KEEPFILTERS(Top3)
    )
```

## Top 5 Products Sales Amount

```DAX
Top 5 Products Sales Amount =
SUMX(
    TOPN(
        5,
        ALL(dimproduct[ItemName]),
        [Sales Amount],
        DESC
    ),
    [Sales Amount]
)
```

## Total Sales Amount

```DAX
Total Sales Amount =
CALCULATE(
    [Sales Amount],
    ALL(dimdate[Year])
)
```

## Visible Product List

```DAX
Visible Product List =
CONCATENATEX(
    VALUES(dimproduct[ItemName]),
    dimproduct[ItemName],
    ", ",
    dimproduct[ItemName]
)
```

## YoY Difference

```DAX
YoY Difference =
[Sales Amount YTD] - [Sales Amount PY]
```

