During testing, we successfully logged into a manager account on the Tomcat service port by using a publicly available brute force exploit. Using an exploit found on Metasploit, we successfully found the credentials 
to the Tomcat Manager account. When this account was compromised, any and all data accessible to the manager is compromised and further attacks can also be conducted (such as uploading a WAR file, 
persistence, privilege escalation, internal network pivoting, etc…) 

After the intial recon phase, I found that port 8180 was hosting the Tomcat service. Upon inputting my target IP and port into FireFox, I was greeted with the home page for the service (Figure 1).

FIGURE 1:

<img width="1867" height="841" alt="Tomcat 1" src="https://github.com/user-attachments/assets/b8d4b1dc-122a-4b69-aa9d-40126d40d3c1" />

Noticing that there is an option to login as a manager (Figure 2), I attempted to do so.

FIGURE 2:

<img width="252" height="220" alt="Tomcat 2" src="https://github.com/user-attachments/assets/57da396d-ad5e-4ec3-b72f-22ae1c4da4a1" />

I was then presented with the windows default page to input credentials for the account. This usually means there is no "lockout" feature to the account after a certain number of failed attempts.

FIGURE 3:

<img width="695" height="439" alt="Tomcat 3" src="https://github.com/user-attachments/assets/05348f34-598c-4961-adc4-e691904ac3cc" />

Since I didn't yet know the credentials, I loaded up Metasploit on my machine (Figure 4).

FIGURE 4:

<img width="348" height="72" alt="Tomcat 4" src="https://github.com/user-attachments/assets/d7c5f494-51a6-482c-8e07-9450a10db35a" />

Then I searched for “tomcat mgr” (Tomcat Manager) to find a module to scan for login credentials on the tomcat manager account. In the case of Figures 5 and 6, it was module 9.

FIGURE 5:

<img width="1902" height="550" alt="Tomcat 5" src="https://github.com/user-attachments/assets/2145219b-4766-46e8-a229-656c5b7f77bb" />

FIGURE 6:

<img width="187" height="51" alt="Tomcat 6" src="https://github.com/user-attachments/assets/abf3b07b-dd99-42e5-a785-0cfd32c070da" />

Upon looking at the options for the module, I noticed that the target IP address and port needed to be specified (Figure 7) to which I did so (Figure 8).

FIGURE 7:

<img width="1895" height="714" alt="Tomcat 7" src="https://github.com/user-attachments/assets/ae935f2d-a9fa-4716-b640-e2434256116a" />

FIGURE 8:

<img width="889" height="132" alt="Tomcat 8" src="https://github.com/user-attachments/assets/2a0bc49b-5954-4f29-8414-006bab4b5fee" />

This module will brute force the manager account by running a combination of username/passwords against the account. Eventually, I found that one of these combinations results in a successful login (Figure 9).

FIGURE 9:

<img width="910" height="248" alt="Tomcat 9" src="https://github.com/user-attachments/assets/b9d47595-14a7-4529-9dd1-80517c63cd33" />

Using the credentials found in Figure 9. I input them into the previous prompt (Figure 10).

FIGURE 10:

<img width="503" height="304" alt="Tomcat 10" src="https://github.com/user-attachments/assets/418a98cb-89b7-4258-9de5-f9ce1574cc49" />

As you can see in Figure 11, this gave me full access to the Tomcat Web Application Manager.

FIGURE 11:

<img width="1728" height="496" alt="Tomcat 11" src="https://github.com/user-attachments/assets/50a20aec-af8b-4359-860c-00ff30e6c618" />


Remediation tips would be as follows:

1. Implement Strong Password Policies

-  Enforce complexity requirements: Ensure passwords contain a mix of upper/lowercase letters, numbers, and special characters.

-  Prohibit commonly used or weak passwords

-  Disable default passwords

2. Require Multi-Factor Authentication (MFA)

-  Implement MFA: Adding an additional layer of security (such as a code sent to a mobile device or a fingerprint scan) can significantly reduce the risk even if a password is weak.

3. Educate Employees

-  Conduct regular training: Teach staff about password security best practices, including recognizing phishing attempts, avoiding password reuse, and creating strong passwords.

-  Promote awareness: Regularly remind employees about the importance of security and the risks associated with weak passwords.

-  Utilize password managers.

