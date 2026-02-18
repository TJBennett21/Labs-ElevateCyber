As you will see below I successfully compromised sensitive login information using a classic SQL injection practice, the sequence of this technique is as follows:

OR 1=1

Only names are exposed and not full credentials. Data was not able to be altered and no systems were crashed or resulted in denying other users access.
However, this vulnerability shows that user input is not properly sanitized, which often means other, more critical data is also at risk.

In Figure 1, I started off on the home page that prompts for a “User ID”

FIGURE 1:

<img width="1107" height="432" alt="SQL Injection 1" src="https://github.com/user-attachments/assets/4901efd6-b5f5-41d1-a497-7adb4b6f276f" />


I started a Burp Suite interceptor and input a simple value such as, “1” into the input box and then pressed “Submit”

FIGURE 2:

<img width="288" height="77" alt="SQL Injection 2" src="https://github.com/user-attachments/assets/ce2c9576-4db2-4fc0-8c31-ae6a918760e7" />


My interceptor captured the GET request that was about to be sent, so now I can view and edit its contents. Seen in Figure 3 as highlighted text, I can add an “OR 1=1” into the request.

FIGURE 3:

<img width="555" height="130" alt="SQL Injection 3" src="https://github.com/user-attachments/assets/d3b19e4c-851b-401f-aeba-daf925abaf17" />


The next step was to URL encode (Ctrl + U) the text of “1 OR 1=1” as shown in Figure 4.

FIGURE 4:

<img width="555" height="132" alt="SQL Injection 4" src="https://github.com/user-attachments/assets/f808f8ce-7e7d-46d7-ba13-4c15104f69e1" />


Finally, I sent this to a replicator to see what the new output would be, and found that multiple user accounts with First and Surnames are shown.

FIGURE 5:

<img width="312" height="373" alt="SQL Injection 5" src="https://github.com/user-attachments/assets/199c08b0-aae0-4517-92b3-768a1e23238c" />


Remediation tips would be as follows:

1. Use Parameterized Queries / Prepared Statements

-These ensure user input is treated as data, not executable code.

2. Input Validation & Allowlisting

-Ensure only expected characters and formats are allowed. 

-For example, usernames shouldn’t contain quotes, SQL keywords, etc.

3. Least Privilege for Database Accounts

-The app’s database user should not have write or admin permissions unless absolutely necessary.

4. Web Application Firewall (WAF)

-Can help detect and block common SQL injection patterns, but it’s a layer of defense — not a substitute for fixing code.

5. Security Testing & Code Review

-Regularly scan the app using tools like Burp Suite, SQLMap, or OWASP ZAP.

-Conduct manual code reviews to identify dynamic SQL statements.
