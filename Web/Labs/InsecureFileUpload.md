During my assessment, I identified a critical vulnerability in the DVWA's file upload functionality that allowed me to upload a malicious file containing executable code. By disguising a small script inside a .php file, I was able to upload it through the application and then access it directly via a browser. 
Once the file was live on the server, I could send custom commands through the browser URL and receive real-time responses from the server — effectively gaining remote command-line access. This demonstrated full control over the system through the web interface, exposing the application to serious risks such as data theft, service disruption, or further unauthorized access.

I started off by creating a new php file on my host machine and labeled it "webshell.php"

FIGURE 1:

<img width="372" height="82" alt="Web File Upload 1" src="https://github.com/user-attachments/assets/ed19a4e2-f0c7-4777-bd87-c6f6a2f18a5c" />

The new file contained the following command found in Figure 2. It allowed me to call on commands from the website after I uploaded the file

FIGURE 2: 

<img width="554" height="118" alt="Web File Upload 2" src="https://github.com/user-attachments/assets/a3287470-429b-4bb8-89f4-b91d6fc11c0c" />

Then, on the web page prompting the user to upload a file, I selected "Choose File" (Figure 3) and proceeded to input my newly created php file (Figure 4)

FIGURE 3: 

<img width="298" height="131" alt="Web File Upload 3" src="https://github.com/user-attachments/assets/111d412b-0563-4e9e-9939-5628b1bea45e" />

FIGURE 4: 

<img width="255" height="139" alt="Web File Upload 4" src="https://github.com/user-attachments/assets/14fc712b-9579-4d97-980c-98089d4721b6" />

The webpage showed me that the file successfully uploaded

FIGURE 5: 

<img width="544" height="157" alt="Web File Upload 5" src="https://github.com/user-attachments/assets/bb70df56-32c5-4cb5-b8c2-b4c2e0f9444e" />

After copying the path out from the webpage (Figure 6), pasting it into the end of the page's URL (Figure 7), and pressing enter, I was brought to a blank page with text at the top of the page displaying a system warning (Figure 8)

FIGURE 6: 

<img width="545" height="171" alt="Web File Upload 6" src="https://github.com/user-attachments/assets/ca5baddb-227c-45ce-a459-14908a37ca62" />

FIGURE 7: 

<img width="490" height="191" alt="Web File Upload 7" src="https://github.com/user-attachments/assets/a9664087-8c75-4a7f-af4b-cb0b69fd2037" />

FIGURE 8:

<img width="922" height="207" alt="Web File Upload 8" src="https://github.com/user-attachments/assets/6e4c46da-9d6c-498a-8ff5-2ee0081ea2b1" />

Now I could call back to the file I uploaded by adding "?command=id" at the end of the URL, to which the webpage would then display the user and group id

FIGURE 9: 

<img width="868" height="125" alt="Web File Upload 9" src="https://github.com/user-attachments/assets/8c469f30-210e-41b6-8e4d-e463a5336990" />

Remediation tips would be as follows:

-Validate file type and extension (Check the file’s actual content  — don’t trust the file extension alone)

-Rename uploaded files (Automatically rename files on the server to prevent execution or directory traversal)

-Scan uploaded files for malware (Use antivirus or malware scanning tools to detect harmful content)

-Restrict file types and size (Only allow required formats (e.g., .jpg, .png) and enforce file size limits)
