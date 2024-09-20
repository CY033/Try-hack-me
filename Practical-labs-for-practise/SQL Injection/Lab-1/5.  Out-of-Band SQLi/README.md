
# Out-of-Band SQL Injection

Out-of-band SQL Injection isn't as common as it depends on specific features being enabled on the database server or the web application's business logic, which may make some kind of external network call based on the results from an SQL query.

An Out-of-Band attack is characterized by having two different communication channels: one to launch the attack and another to gather the results. For example, the attack channel could be a web request, and the data-gathering channel could involve monitoring HTTP/DNS requests made to a service you control.

1. An attacker makes a request to a website vulnerable to SQL Injection with an injection payload.
2. The website makes an SQL query to the database, which also passes the hacker's payload.
3. The payload contains a request that forces an HTTP request back to the hacker's machine containing data from the database.


![75103b88e95d4eda2244dbb360cba4ff](https://github.com/user-attachments/assets/6e124425-dae5-4dcc-96d6-e6a6a7f5bd45)


## Questions

Name a protocol beginning with D that can be used to exfiltrate data from a database.  
**Answer format:** ***
```

```
<details>
<summary>Click to see answer</summary>
 

```python
DNS
```
