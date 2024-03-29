# T-SQL Dynamic Pivot
### How to Pivot SQL Data dynamically

### Jerry Payne


I always find the need to pivot data, but don't always know the data that I need to pivot.  Currently, with TSQL you have to 'Explicitly' list the Data that you want, as column names.  But what if you don't know, or what if there is a new entry in the table that you need to pivot.  This has plagued me for years, and I have finally found a solution.  

First, I will list the functions I used.  Then we can create a table, and insert some data.

**STUFF()** Function:
https://docs.microsoft.com/en-us/sql/t-sql/functions/stuff-transact-sql?view=sql-server-ver15
```SQL
STUFF ( character_expression, 
start , length , replaceWith_expression )
```

**QUOTENAME()** Function:
https://docs.microsoft.com/en-us/sql/t-sql/functions/quotename-transact-sql?view=sql-server-ver15
```SQL
QUOTENAME ( 'character_string' [ , 'quote_character' ] )
```

**FOR XML()** Function:
https://docs.microsoft.com/en-us/sql/relational-databases/xml/for-xml-sql-server?view=sql-server-ver15


CREATE TABLE
```SQL
CREATE TABLE dbo.PivotTest
(
UnitNumber VARCHAR(10) NOT NULL,
PayrollDate DATE NOT NULL,
GrossRevenue DECIMAL(9,2) NOT NULL 
);
```




INSERT
```SQL
INSERT INTO dbo.PivotTest
(UnitNumber, PayrollDate, GrossRevenue)
VALUES
('111','1/1/2019','1000'),
('222','1/1/2019','2000'),
('111','2/1/2019','3000'),
('222','2/1/2019','4000'),
('111','3/1/2019','1000'),
('222','3/1/2019','5000'),
('111','4/1/2019','3000'),
('222','4/1/2019','2000');

```



Get the Distinct values to use for Column Headers.
```SQL
SELECT 
    STUFF(
        (SELECT DISTINCT ', ' + 
        QUOTENAME([PayrollDate]) 
FROM 
    dbo.PivotTest FOR XML PATH ('')),1,2,'')
```

Another option, if you want to sort the Column Headers.
```SQL
SELECT
    STUFF(
        (SELECT ', ' +
         QUOTENAME([PayrollDate]) 
FROM 
	dbo.PivotTest
WHERE    --Filter Data
	[PayrollDate] > '12/28/2018'
GROUP BY --Used in place of DISTINCT
	[PayrollDate]
ORDER BY --Order the Columns
	[PayrollDate]
FOR XML PATH ('')),1,2,'')

