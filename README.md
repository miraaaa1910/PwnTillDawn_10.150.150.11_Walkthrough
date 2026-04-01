# PwnTillDawn_10.150.150.11_Walkthrough
Lab 3 Vulnerability Analysis

I ran OpenVPN, it shows the version and warns about compression. Then it connects to the VPN server on port 443 and establishes a secure TLS handshake. The certificates are verified successfully, and encryption is set with AES 256. The server gives me my VPN IP and routing details. The tunnel interface comes online, routes are added, and finally I see ‘Initialization Sequence Completed.’ That means the VPN is fully connected and I’m inside the lab network, ready to begin testing.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20204917.png )

I ran an Nmap scan on the target 10.150.150.11 with service and script detection enabled. The scan completes and shows several open ports: 80 and 443 for web, 135, 139, 445 for Windows services, 3389 for RDP, 3306 for MySQL, and 1433 for MSSQL, plus some high numbered dynamic ports. This tells me the host is likely a Windows machine with both web and database services exposed, giving me multiple attack paths to explore.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20222609.png )
![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20222530.png )

I ran an Nmap vuln scan on ports 135, 139, and 445 of the target. It shows the SMB services are open, and the results confirm the host is vulnerable to MS17 010, also known as EternalBlue. That’s a high risk remote code execution flaw in SMBv1, the same one used in major attacks like WannaCry. This gives me a clear exploitation path to gain access.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20210724.png )

I launch Metasploit with msfconsole. The console loads up, shows a quick tip about searching exploits, and even displays some playful ASCII art. Then it prints the version details Metasploit v6.4.99 along with the number of exploits, payloads, and modules available. This confirms the framework is ready, giving me access to thousands of tools for exploitation and post exploitation work.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20211230.png )

Inside Metasploit, I search for ms17 010. The results list several exploit modules, including EternalBlue and EternalRomance, all targeting the SMB vulnerability. This confirms I have multiple options to exploit the Windows host, with EternalBlue being the most direct path for remote code execution.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20211241.png )

I set the target host and my local listener, then run the EternalBlue exploit. Metasploit checks the SMB service and confirms the system is vulnerable. It connects, sends the crafted packets, triggers the buffer overflow, and finally delivers the payload. The exploit succeeds, and I get a Meterpreter session opened on the target full remote access to the Kali machine.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20221807.png )

After exploiting EternalBlue, I drop into a Meterpreter session and open a shell. The target responds as Windows Server 2008 R2. I navigate into the Users directory and list accounts. I see Administrator, jboden, tony, and others. This confirms I have interactive access on the system and can start exploring user files or escalating privileges further.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20221831.png )

Inside the shell, I move into the Administrator folder and run dir. It shows the usual Windows profile directories like Desktop, Documents, Downloads, Music, Pictures, and more. This confirms I have full access to the Administrator’s home directory, meaning I can explore files, gather data, or drop persistence from here.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20221847.png )

I move into the Administrator’s Desktop folder and run dir. The listing shows two files: a shortcut for Xlight FTP Server and a text file named FLAG1.txt. Finding the flag here confirms successful exploitation and access, marking the first objective completed.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20221856.png )

I navigate into the Administrator’s Desktop and open the FLAG1.txt file. Inside, it reads ‘PwnTillDawnAcademyIsAwesome!!!’. That confirms I’ve successfully exploited the target, gained access, and captured the first flag.

![image]( https://github.com/miraaaa1910/PwnTillDawn_10.150.150.11_Walkthrough/blob/59a8cdacbc704eb6f70c1965bbb230a54fff5e91/Screenshot%202026-04-01%20221905.png )
