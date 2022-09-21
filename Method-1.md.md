# Active Directory Penetration testing
## Step 1: Perform Nmap scan
~~~
nmap -sV -sC <ip_address>
~~~
Take a look at the services open, if smb is present:
~~~
sudo smbclient \\\\<ip_address>\\\\
~~~
- If you get **permission enied** message then remove password and try.
- mount share drive:
~~~
cd /mnt/share/
~~~
- Above step will work only if no authentication method is implemented for share drive.
- Enumerate folders by checking each folder. Check if any password is stored in plain text.

# Step 2: Login into AD
- create a username text file.
- if smb is available use **crackmapexec** to brute force into SMB:
~~~
sudo crackmapexec smb <ip_address> -u <username list file> -p <password>
~~~
- Use evil-winrm to login to non Privileage account:
~~~
sudo ruby evil-winrm -i <ip_address> -u <username from crackmapexec> -p <password>
~~~

# Step 3: Privileage Escalation
- Enumerate all users and groups. E.g. scripts are in OSCP.
- Enumerate members of Domain group.
- Run winPEAS.
- If any clean text password present, run crackmapexec on it to find user:
~~~
sudo crackmapexec smb <ip_number> -u <userlist> -p <password>
~~~
- if no results are found, trying enclosing password in **single** or **double** quotes.
- Try to login with new user from evil-winrm:
~~~
sudo evil-winrm -i <ip_address> -u <username> -p <password>
~~~
- Use hashcat to dump passwords and login as administrator with evil-winrm.