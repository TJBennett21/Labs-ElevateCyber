During testing, I identified a critical OS command injection vulnerability caused by improper input validation in a user-controlled field within the application. Using Burp Suite, I intercepted and modified a GET request 
to inject system commands directly into the server-side process. By appending a command such as ;id to the input parameter, I was able to execute arbitrary commands on the underlying system and confirm 
the user context of the web server. This confirmed that an attacker could potentially achieve full system compromise through unsanitized input handling.

As seen (Figure 1), starting off on the homepage of the website I was prompted to input an IP address. This is the intended function of the site.

FIGURE 1:

<img width="1249" height="491" alt="Command Injection 1" src="https://github.com/user-attachments/assets/dc8cf25d-ce6f-4e61-83f7-be9c3e1e8bf0" />

By adding a command after my input (Figure 2), I was able to gain unintended access to the system. Simply by adding “;id” to the end of the line, an attacker is able to see what user ID is being used. 
This was just an example command. Other commands to gain further access or different information could be utilized by a Threat Actor.

FIGURE 2:

<img width="539" height="265" alt="Command Injection 2" src="https://github.com/user-attachments/assets/943153ab-f3ea-4337-a0e3-36fcdcf1f586" />

Remediation tips would be as follows:

1. Use input validation and allowlisting

-  Only accept known and safe input formats (e.g., valid IP addresses or usernames).

-  Reject unexpected characters such as ;, &, |, or other special shell metacharacters.

2. Avoid using shell commands with user input

-  Do not pass user input directly into system-level commands (exec(), system(), etc.).

-  Use safe language-native alternatives (e.g., filesystem libraries, network APIs) that don’t invoke a shell.

3. Implement least privilege for backend services

-  Run the web application with the lowest possible privileges.

-  Prevent access to sensitive files or commands even if exploitation occurs.

4. Log and monitor command activity

-  Continuously log application-level command executions.

-  Set up alerts for suspicious or abnormal command behavior to detect possible attacks early.
