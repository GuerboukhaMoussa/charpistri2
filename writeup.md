# Write-up: FCSC 2022 - Web Challenge - Chatpristi 2

## Challenge Description
The challenge consisted of a simple web application featuring a collection of cat memes and a search bar. As soon as I accessed the website, I noticed the search bar, which immediately caught my attention as a possible entry point for injection attacks.

---

## Reconnaissance
I started by exploring the web page to understand its layout and possible vulnerabilities. The search bar seemed like a great candidate to test for SQL Injection vulnerabilities, as such fields often interact with the database.

---

## Testing Inputs

### First Test - Single Quote (')
To confirm any possible vulnerabilities, I began testing the search bar with random inputs.  
I entered a single quote (`'`) to test for SQL injection vulnerability. The result was an error message:

```sql
pq: operator is not unique: unknown % unknown
