```markdown
# In-Band SQL Injection

In-Band SQL Injection is the easiest type to detect and exploit. "In-Band" refers to using the same method of communication to exploit the vulnerability and receive results. For example, discovering an SQL Injection vulnerability on a website page and then being able to extract data from the database to the same page.

## Error-Based SQL Injection

This type of SQL Injection is the most useful for easily obtaining information about the database structure, as error messages from the database are printed directly to the browser screen. This can often be used to enumerate a whole database.

## Union-Based SQL Injection

This type of injection utilizes the SQL UNION operator alongside a SELECT statement to return additional results to the page. This method is the most common way of extracting large amounts of data via an SQL Injection vulnerability.

## Practical Exercise

Click the green "Start Machine" button to use the SQL Injection Example practice lab. Each level contains a mock browser and SQL Query and Error boxes to assist in getting your queries/payload correct.

Level one of the practice lab features a mock browser and a website showcasing a blog with different articles, which can be accessed by changing the ID number in the query string.

### Discovering Error-Based SQL Injection

The key to discovering error-based SQL Injection is to break the SQL query by trying certain characters until an error message is produced; these are most commonly single apostrophes (`'`) or quotation marks (`"`).

1. Try typing an apostrophe (`'`) after `id=1` and press enter. You should see an SQL error message indicating an error in your syntax. The fact that you've received this error confirms the existence of an SQL Injection vulnerability.

2. We can now exploit this vulnerability and use the error messages to learn more about the database structure. 

### Returning Data without Error Messages

First, we'll try the UNION operator to receive an extra result. Set the mock browser's ID parameter to:

```sql
1 UNION SELECT 1
```

This statement should produce an error message indicating that the UNION SELECT statement has a different number of columns than the original SELECT query. 

3. Try again by adding another column:

```sql
1 UNION SELECT 1,2
```

4. Same error again, so let's add another column:

```sql
1 UNION SELECT 1,2,3
```

5. Success! The error message has disappeared, and the article is being displayed. However, we want to display our data instead of the article. To do this, we need the first query to produce no results. Change the article ID from 1 to 0:

```sql
0 UNION SELECT 1,2,3
```

Now you'll see the article is just made up of the result from the UNION select, returning the column values 1, 2, and 3. We can start using these returned values to retrieve more useful information.

### Getting Database Information

To get the database name we have access to, run the following query:

```sql
0 UNION SELECT 1,2,database()
```

You'll now see where the number 3 was previously displayed; it now shows the name of the database, which is `sqli_one`.

### Gathering Table Names

Next, let's gather a list of tables in this database:

```sql
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
```

- The `group_concat()` method gets the specified column (in our case, `table_name`) from multiple returned rows and concatenates them into one string separated by commas.
- The `information_schema` database contains information about all the databases and tables accessible to the user. In this query, we want to list all tables in the `sqli_one` database: `article` and `staff_users`.

### Finding Table Structure

As the first level aims to discover Martin's password, the `staff_users` table is of interest. To find the structure of this table, use the following query:

```sql
0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
```

This query retrieves the column names from the `staff_users` table. The results provide three columns: `id`, `password`, and `username`. We can use the `username` and `password` columns for our next query.

### Retrieving User Information

To retrieve the user's information, run the following query:

```sql
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
```

Here, we use `group_concat()` to return all rows into one string, using `': '` to separate usernames and passwords. Instead of commas, we use the HTML `<br>` tag to format each result on a separate line for easier reading.

You should now have access to Martin's password to enter and move to the next level.
```
