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

<img width="689" height="519" alt="CSRF 0" src="https://github.com/user-attachments/assets/78663a5a-fbfb-4897-a065-4e428b539de1" />

Then, I created a new file on my machine (Kali Linux) to contain a malicious script and named it “index.html”


FIGURE 3:

<img width="370" height="86" alt="CSRF 1" src="https://github.com/user-attachments/assets/2f861cfa-92bc-4875-bb35-947fec143608" />



FIGURE 4:

FIGURE 5:

FIGURE 6:

FIGURE 7:

FIGURE 8: 

FIGURE 9:

