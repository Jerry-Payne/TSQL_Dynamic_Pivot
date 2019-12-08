# TSQL Dynamic Pivot
### How to Pivot SQL Data dynamically

I always find the need to pivot data, but don't always know the data that I need to pivot.  Currently, with TSQL you have to 'Explicitly' list the Data that you want as column names that you want to pivot.  But what if you don't know, or what if there is a new entry in the table that you need to pivot.  This has plagued me for years, and I have finally found a solution.  

First, I will list the functions I used.  Then we can create a table, and insert some data.

**STUFF** Function:
https://docs.microsoft.com/en-us/sql/t-sql/functions/stuff-transact-sql?view=sql-server-ver15
```SQL
STUFF ( character_expression, 
start , length , replaceWith_expression )
```

**QUOTENAME** Function:
https://docs.microsoft.com/en-us/sql/t-sql/functions/quotename-transact-sql?view=sql-server-ver15
```SQL
QUOTENAME ( 'character_string' [ , 'quote_character' ] )
```

**FOR XML** Function:
https://docs.microsoft.com/en-us/sql/relational-databases/xml/for-xml-sql-server?view=sql-server-ver15


Create table
```SQL
CREATE TABLE dbo.PivotTest
(
UnitNumber VARCHAR(10) NOT NULL,
PayrollDate DATE NOT NULL,
GrossRevenue INT NOT NULL 
);
```




Insert Data
```SQL
INSERT INTO
```




```SQL
SELECT 
    STUFF(
        (SELECT DISTINCT ', ' + 
        QUOTENAME([PayrollEndingPeriod]) 
FROM 
    Dispatch.TruckExpense FOR XML PATH ('')),1,2,'')
```
