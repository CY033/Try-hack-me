
# Remediation

As impactful as SQL Injection vulnerabilities are, developers can protect their web applications by following the advice below:

## Prepared Statements (With Parameterized Queries)

In a prepared query, the first thing a developer writes is the SQL query, and then any user inputs are added as parameters afterwards. Writing prepared statements ensures the SQL code structure doesn't change, allowing the database to distinguish between the query and the data. This also makes your code cleaner and easier to read.

## Input Validation

Input validation can significantly enhance security by restricting what can be included in an SQL query. Employing an allow list can limit input to only certain strings, or a string replacement method in the programming language can filter the characters you wish to allow or disallow.

## Escaping User Input

Allowing user input to contain characters such as `'`, `"`, `$`, or `\` can lead to SQL queries breaking or, worse, being vulnerable to injection attacks. Escaping user input involves prepending a backslash (`\`) to these characters, causing them to be parsed as regular strings rather than special characters.

## Questions

Name a method of protecting yourself from an SQL Injection exploit.  
**Answer format:** ******** **********

<details>
<summary>Click to see answer</summary>

```sql
prepared statements
```
</details>
