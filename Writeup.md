# Hydra Challenge Writeup (TryHackMe)
#pentest/tryhackme

The first thing we need to do is connect to the TryHackMe VPN. Here's how:

1. Sign in to your TryHackMe account and go to the 'Access' page.
2. Download your VPN configuration file.
3. Install a VPN client like OpenVPN.
4. Import the configuration file into the VPN client.
5. Connect to the VPN using the imported configuration.

Once connected, you can start accessing the TryHackMe labs and exercises.

# Nmap scan
Next, we'll perform an Nmap scan by typing the following command in the shell

```shell
$ sudo nmap -sV -O <IP>
```

The scan results will show that port 80 (HTTP) and the 22 (SSH) port are open.

# First Task: Brute Forcing the Web Password
