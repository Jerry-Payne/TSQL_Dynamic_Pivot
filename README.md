# TSQL Dynamic Pivot
How to Pivot SQL Data dynamically

This has plagued me for years.  I always find the need to pivot data, but don't always know the data that I need to pivot.

STUFF Function:
https://docs.microsoft.com/en-us/sql/t-sql/functions/stuff-transact-sql?view=sql-server-ver15
```SQL
STUFF ( character_expression, 
start , length , replaceWith_expression )
```

QUOTENAME Function:
https://docs.microsoft.com/en-us/sql/t-sql/functions/quotename-transact-sql?view=sql-server-ver15
```SQL
QUOTENAME ( 'character_string' [ , 'quote_character' ] )
```

FOR XML Function:
https://docs.microsoft.com/en-us/sql/relational-databases/xml/for-xml-sql-server?view=sql-server-ver15
```SQL
QUOTENAME ( 'character_string' [ , 'quote_character' ] )
```



```SQL
SELECT 
    STUFF(
        (SELECT DISTINCT ', ' + 
        QUOTENAME([PayrollEndingPeriod]) 
FROM 
    Dispatch.TruckExpense FOR XML PATH ('')),1,2,'')
```
