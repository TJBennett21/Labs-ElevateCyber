I identified a weak/default credential vulnerability caused by the absence of strong password policy enforcement within the application. Using a publicly available password list from SecLists 
and intercepting login requests with Burp Suite, I performed a brute-force attack against the login functionality. By automating credential attempts with a list of commonly used passwords, I successfully 
discovered valid credentials and gained administrative access to the system. This demonstrated that the application lacks effective protections against brute-force attacks, allowing an attacker to obtain 
unauthorized access to sensitive accounts. Additionally, the login request was transmitted using a GET method, which further exposed credentials and made request interception and manipulation easier.

To begin testing, I obtained a password list by downloading the SecLists repository onto my machine. This repository contains multiple collections of commonly used and default credentials that can 
be leveraged when testing authentication security.

FIGURE 1:

<img width="1075" height="299" alt="Web Brute Force 1" src="https://github.com/user-attachments/assets/62e73b01-b410-4c88-b2e1-5636fb3d2b94" />

On the website’s login page, I entered a known username along with a random password to observe the system’s response. The application returned an “incorrect password” message, while HTTP traffic 
was simultaneously captured using Burp Suite in the background.

FIGURE 2:

<img width="312" height="199" alt="Web Brute Force 2" src="https://github.com/user-attachments/assets/9435f9f8-d1a4-4c3e-907f-f0e44786e902" />

The intercepted request revealed the submitted credentials within the HTTP parameters. In the highlighted section of the request, the username and password fields were visible, allowing me to 
identify exactly where the input values could be modified during testing.

FIGURE 3:

<img width="664" height="458" alt="Web Brute Force 3" src="https://github.com/user-attachments/assets/e5e030ff-0124-429f-8173-bb6ed3d7d337" />

While reviewing the password lists contained within the SecLists repository, I selected the Common-Credentials directory for this attack scenario. From that directory, the 500-worst-passwords.txt 
list was chosen because it contains a large collection of widely used and easily guessable passwords.

FIGURE 4:

<img width="1890" height="446" alt="Web Brute Force 4" src="https://github.com/user-attachments/assets/8857cd67-d478-4f11-96a9-e73acec18599" />

Within Burp Suite, the password parameter captured earlier was highlighted and added as an attack position. After selecting the Add option, the request was configured as shown in Figure 6, indicating 
that the password field had been successfully marked for automated payload injection.

FIGURE 5:

<img width="1425" height="230" alt="Web Brute Force 5" src="https://github.com/user-attachments/assets/d6c0f5e9-64ed-44d9-b936-c44e65e73ae4" />

Next, I navigated to the Payloads tab within Burp Suite. From the payload settings menu, the Load option was used to import the 500-worst-passwords.txt file from the Common-Credentials folder 
of the SecLists repository.

FIGURE 6:

<img width="683" height="433" alt="Web Brute Force 6" src="https://github.com/user-attachments/assets/f4721fad-088e-4dc7-b54e-fc74f0c5bbe8" />

Once the payload list had been configured, the Start Attack button was selected in the top-right corner of the interface. This action initiated the automated brute-force process against the login request.

FIGURE 7:

<img width="1558" height="381" alt="Web Brute Force 7" src="https://github.com/user-attachments/assets/4f38a642-93ac-41e1-95ce-80b4844711bb" />

During the attack, each password in the list was tested while keeping the username constant. One of the responses returned a successful login, confirming that valid credentials had been discovered 
through the brute-force attempt.

FIGURE 8:

<img width="1172" height="603" alt="Web Brute Force 8" src="https://github.com/user-attachments/assets/1c59a48d-3c28-42c9-a093-15f83cd8fb5f" />

An additional observation was that the authentication request used the HTTP GET method rather than POST. Because the credentials were transmitted within the URL parameters, this design flaw allowed me 
to easily capture and modify the request during testing, which facilitated the brute-force attack.

(BONUS) FIGURE 9:

<img width="648" height="108" alt="Web Brute Force GET request vulnerability" src="https://github.com/user-attachments/assets/992986ae-cc7d-4277-9657-e5110bebcff0" />

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


