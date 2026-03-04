For this assessment, I identified an exposed Java RMI service running on TCP port 1099 with an insecure default configuration that allowed unauthenticated remote interaction. Using Metasploit, I enumerated 
the RMI endpoint and utilized a publicly available exploit module targeting insecure RMI configurations. Through this, I achieved remote code execution within the JVM and obtained root-level access to 
the underlying host. This demonstrated that the RMI registry was improperly secured, resulting in full system compromise and a complete loss of Confidentiality, Integrity, and Availability. 


I identified an active Java RMI service listening on TCP port 1099 (Figure 1), confirming that the RMI registry was exposed and accessible.

FIGURE 1:

<img width="256" height="70" alt="JavaRMI 0" src="https://github.com/user-attachments/assets/a6f4995b-ff3a-4bb3-baae-8cc7df6fd7ab" />

To begin exploitation, I launched the Metasploit console on my host machine using the msfconsole command (Figure 2).

FIGURE 2:

<img width="343" height="68" alt="JavaRMI 1" src="https://github.com/user-attachments/assets/dca21614-b2d2-4c61-87e6-6227ce1fa9ba" />

Within Metasploit, I searched for auxiliary modules related to Java RMI to identify available enumeration options.

FIGURE 3:

<img width="385" height="63" alt="JavaRMI 2" src="https://github.com/user-attachments/assets/ff4084cc-e91e-4abe-9c03-6e0274e4794f" />

From the results returned, I selected the auxiliary module designed to detect insecure RMI endpoints vulnerable to code execution.

FIGURE 4:

<img width="1779" height="273" alt="JavaRMI 3" src="https://github.com/user-attachments/assets/1676c592-d551-4a88-8854-96d3ed1c2040" />

Using the second auxiliary module, the selected module was loaded into the active workspace.

FIGURE 5:

<img width="171" height="45" alt="JavaRMI 4" src="https://github.com/user-attachments/assets/f089b76b-e3a5-476c-838c-76feff6ba831" />

After displaying the module options, I confirmed that the RHOSTS parameter needed to be defined with the target IP address.

FIGURE 6:

<img width="1845" height="262" alt="JavaRMI 5" src="https://github.com/user-attachments/assets/bde32c11-3fb4-4754-bd27-7b8861c23fe7" />

I then configured the module by setting the RHOSTS value to my target IP address.

FIGURE 7:

<img width="876" height="71" alt="JavaRMI 6" src="https://github.com/user-attachments/assets/da36906a-53f1-46b5-9ca0-4b105ca84a72" />

Once executed, the module successfully identified an exposed RMI endpoint on the target system.

FIGURE 8:

<img width="1186" height="116" alt="JavaRMI 7" src="https://github.com/user-attachments/assets/e62e4d01-f7c5-4716-8098-dca03b9df954" />

With enumeration complete, I then proceeded to search for a suitable exploit module targeting the identified RMI vulnerability.

FIGURE 9:

<img width="941" height="37" alt="JavaRMI 8" src="https://github.com/user-attachments/assets/1b261c6e-ffd2-4e71-909a-d455f00241bc" />

Among the available options, I selected the exploit module addressing insecure default RMI configurations (module 7).

FIGURE 10:

<img width="1903" height="201" alt="JavaRMI 9" src="https://github.com/user-attachments/assets/2fc9790d-f8cf-4dcc-9c6f-b4b19f617507" />

I selected the 7th exploit module (Figure 11).

FIGURE 11:

<img width="908" height="70" alt="JavaRMI 10" src="https://github.com/user-attachments/assets/5f7d9e24-8cf9-47aa-a6ad-05b165cb790c" />

Reviewing the module’s configuration settings confirmed that the RHOSTS parameter again needed to be specified.

FIGURE 12:

<img width="1882" height="423" alt="JavaRMI 11" src="https://github.com/user-attachments/assets/5b0e1dff-cfa1-417a-b108-3b411b383d19" />

After setting the RHOSTS value to the target system’s IP address, the exploit was fully configured.

FIGURE 13:

<img width="826" height="68" alt="JavaRMI 12" src="https://github.com/user-attachments/assets/963fcd05-f887-4de9-924d-bafa0ef1b5e1" />

Following execution of the exploit, I verified successful compromise by running the getuid command, which confirmed that I had obtained root-level access to the system.

FIGURE 14:

<img width="201" height="47" alt="JavaRMI 13" src="https://github.com/user-attachments/assets/a81da416-8bea-4299-8b2e-b7d8e54f0663" />


Remediation tips would be as follows:

1. Don’t expose RMI services to untrusted networks

-  Restrict RMI ports using host-based firewalls, security groups, or network ACLs.

2. Run RMI services with minimal privileges

-  Follow the principle of least privilege—run the JVM and application as a non-root user with limited permissions.

3. Implement RMI authentication and access controls

-  Enforce security policies and implement RMI security managers.
