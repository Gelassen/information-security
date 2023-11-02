Detecting First or Second-Order SQL Injections

    Identify the APIs used to execute dynamic SQL statements.
    Review whether any data validation is performed on the data used in the dynamic SQL statement.
    If data validation is not performed, review whether the data is escaped for delimiting characters (single quotes for string literals and right square brackets for SQL identifiers).

Detecting SQL Modification by Truncation Issues

    Review the buffer lengths used that are to store the final dynamic SQL statement.
    Calculate the maximum buffer required to hold the SQL statement when the input is maxed out and see if the buffer used to hold the SQL statement is big enough.
    Pay special attention to the return values from QUOTENAME or REPLACE functions; if the length of input data is n characters, these functions will return 2*n + 2 or 2*n when all the input characters are delimiting characters.
    For C/C++ applications, check whether the return values from APIs like StringCchPrintf that are used to prepare the SQL statement are checked for insufficient buffer errors.

Detecting SQL Injection by Truncation Issues

    Review the buffer lengths that are used to store the delimited strings or escaped strings.
    If n is the length of the input string, then you will need 2*n + 2 for storing the return value from QUOTENAME and 2*n for storing the return value from REPLACE.
    For C/C++ applications, check whether the return values from replace equivalent functions are checked for insufficient buffer errors.

Using Black Box Methods If you have automated tools or smart fuzzers, then here are some techniques for detecting issues in your SQL statements.

Detecting SQL Injection Issues

    Send single quotes as the input data to catch the instances where the user input is not sanitized and used as string literals inside a dynamic SQL statement.
    Use right square brackets (the ] character) as the input data to catch instances where the user input is used as part of a SQL identifier without any input sanitization.

Detecting Truncation Issues

    Send long strings, just as you would send strings to detect buffer overruns.

Detecting SQL Modification by Truncation Issues

    Send long strings of single quote characters (or right square brackets or double quotes). These will max out the return values from REPLACE and QUOTENAME functions and might truncate the command variable used to hold the SQL statement.

Source <a href="https://learn.microsoft.com/en-us/archive/msdn-magazine/2006/november/sql-security-new-sql-truncation-attacks-and-how-to-avoid-them">https://learn.microsoft.com/en-us/archive/msdn-magazine/2006/november/sql-security-new-sql-truncation-attacks-and-how-to-avoid-them</a>