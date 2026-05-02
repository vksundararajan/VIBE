+++
title = "TryHackMe Writeup — Billing"
date = "2025-04-18"
tags = ["ctf","security"]
+++

`Some mistakes can be costly. Gain a shell, find the way and escalate your privileges! Bruteforcing is out of scope for this room.`

Room Link: [https://tryhackme.com/room/billing](https://tryhackme.com/room/billing)

### 1. Enumeration
![Image](/images/thm-billing/1.png)

Identifying whether a target machine operates on **Linux** can be facilitated by examining the TTL (Time to Live) value during a ping test. A TTL value of 63 typically indicates a Linux system, enabling a more tailored enumeration strategy that is.

![Image](/images/thm-billing/2.png)

The results from the port scanning with **Threader3000** indicate the presence of four open ports. The common services identified include SSH, HTTP, and MySQL, in addition to an additional atypical port, 5038.

![Image](/images/thm-billing/3.png)

Analyzing the data, it is evident that the uncommon port is operating **Telnet**; however, this discovery does not advance our progress significantly. Further investigation of **HTTP** could yield more insights about the target.

![Image](/images/thm-billing/4.avif)

Upon conducting a comprehensive Nmap scan on the HTTP service, it was identified that the HTTP title is **MagnusBilling**. A vulnerability has been identified in this service — **CVE-2023-30258**

### 2. Exploitation

![Image](/images/thm-billing/5.avif)

Based on the command mentioned in the [GitHub - CVE-2023-30258](https://github.com/Chocapikk/CVE-2023-30258), we can ascertain that the command was successfully executed on the target system. However, the results were not evident. Nevertheless, this process can facilitate establishing

![Image](/images/thm-billing/6.png)

A reverse shell has been obtained, allowing access to the Asterisk user's shell. This access will facilitate the escalation of privileges to the Root user.

![Image](/images/thm-billing/7.png)

Once the user-level access was achieved, retrieved the user flag and focus now shifts to obtaining Root.

### 3. Privilege Escalation

![Image](/images/thm-billing/8.png)


The user asterisk has been granted privileges to execute the fail2ban-client command with root permissions. This action does not require a password. The **fail2ban-client** is a command-line tool used to manage and control the **fail2ban-server**. Fail2ban enhances security by monitoring log files for suspicious activities, such as multiple failed login attempts, and mitigates these
![Image](/images/thm-billing/9.avif)

Upon examining the status of the **fail2ban-server**, there are currently 8 active jails. Each jail serves as a configuration specifying the logs to monitor, the patterns to identify, and the corresponding actions to execute when these patterns are detected.

![Image](/images/thm-billing/10.avif)

Initially, retrieve the current actions for an active jail. Proceed by modifying the **actionban** command within the **iptables-allports-ASTERISK** action, executed when banning an IP for the **asterisk-iptables** jail. Configure it to execute a command that sets the **setuid** 

![Image](/images/thm-billing/11.avif)

Then banning an IP address for the **asterisk-iptables** jail involves executing the command specified for actionban in the **iptables-allports-ASTERISK** action configuration.

![Image](/images/thm-billing/12.png)

So, that’s it ah!! Once the setuid bit is set on the **/bin/bash** binary, we can execute it to gain shell access, allowing us to read the root flag, thus completing this challenge.

---

Thanks for sticking with me through the journey!





