During testing, I successfully executed a Cross-Site Request Forgery (CSRF) attack by crafting a malicious HTML payload that performed an unauthorized password change on the target application. Using Burp Suite’s 
HTTP Intercept tool, I then embedded this URL inside an image tag in an HTML file:

<img src="http://10.0.17.235/vulnerabilities/csrf/?password_new=hacker&password_conf=hacker&Change=Change" />

When this file was opened in a browser by an authenticated user, the request was silently sent in the background, successfully changing the user’s password without their consent — proving that the application failed 
to validate the origin or intent of the request.

Starting off on the homepage to change your password as a user, I was prompted to set and confirm a new password. This will simulate a user within a system/company creating a new password. Ensuring I had an HTTP traffic 
monitor/receiver active, I input a random password.

FIGURE 1:

<img width="303" height="169" alt="CSRF negative 1" src="https://github.com/user-attachments/assets/f0b67c67-47b6-4539-95a8-7e91c9917e55" />

In the details of the previous GET request, we can see what example password that I input in Figure 1. This represents the command running against the webpage.

FIGURE 2:

<img width="686" height="525" alt="CSRF 0" src="https://github.com/user-attachments/assets/cea7ee65-a8e1-4b0e-a474-b07e0a28d563" />

Then, I created a new file on my machine (Kali Linux) to contain a malicious script and named it “index.html”

FIGURE 3:

<img width="370" height="86" alt="CSRF 1" src="https://github.com/user-attachments/assets/2f861cfa-92bc-4875-bb35-947fec143608" />

Inserting the script seen in Figure 4, this changes the GET request material by manipulating what password I had input in the previous figures. The password will change from my input password to “hacker”

FIGURE 4:

<img width="1486" height="70" alt="CSRF 2" src="https://github.com/user-attachments/assets/8f337b7f-67b7-4cac-9aa2-17da48ddfdb5" />

Now, I created a temporary python web server to simulate a separate link or webpage that the target user would have open. I clicked the http link that it provides (this can be seen in the red box in Figure 5)

FIGURE 5:

<img width="810" height="181" alt="CSRF 3" src="https://github.com/user-attachments/assets/5524e2d5-4930-4b9c-b511-b56483d61295" />

Clicking the link located in Figure 5 brought me to a blank web page. This is simply simulating a different web page that the victim will have opened. 

FIGURE 6:

<img width="1362" height="830" alt="CSRF 4" src="https://github.com/user-attachments/assets/f85ad68b-461a-4c17-8252-6e56bb36876e" />

Going back to my previously seen GET request, I sent this to my Burp Suite “Repeater” tab to replicate this request.

FIGURE 7:

<img width="1051" height="347" alt="CSRF 5" src="https://github.com/user-attachments/assets/04b7ffcd-29af-4ca6-8b89-0888b25657ac" />

After sending the request to the Repeater, I sent this repeat request (located in Figure 8).

FIGURE 8: 

<img width="658" height="485" alt="CSRF 6" src="https://github.com/user-attachments/assets/af3ba169-09d3-44bb-85c3-7ed93c8b1bf7" />

Simulating this new request will result in the password being changed to “hacker” as the attacker intended.

FIGURE 9:

<img width="551" height="415" alt="CSRF 7" src="https://github.com/user-attachments/assets/af6aec49-e6a6-469e-9c3c-1c710292525b" />

Remediation tips would be as follows:

1. Implement CSRF Tokens

-  Use secure, unpredictable CSRF tokens in all state-changing forms or requests.

-  Verify the token on the server before processing the request.

2. Enforce SameSite Cookies

-  Set cookies with the SameSite=Strict or SameSite=Lax attribute to block cookies from being sent in cross-origin requests.

3. Validate Request Origin

-  Check the Origin or Referer header to ensure requests are coming from trusted sources only.

4. Require Re-authentication for Sensitive Actions

-  Prompt users to re-enter their password before changing critical account settings such as password, email, or payment info.
