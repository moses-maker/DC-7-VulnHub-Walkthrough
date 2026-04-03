# DC-7-VulnHub-Walkthrough
Full walkthrough of VulnHub's DC-7 CTF machine – from reconnaissance to root. Includes enumeration, credential discovery, Drupal PHP filter exploitation, cron job abuse, and privilege escalation.

DC-7-VulnHub-Walkthrough/
│
├── README.md                 # Main writeup
├── findings.md               # Structured findings / observation notes
├── screenshots/              # All images (rename your media files)
│   ├── ifconfig.png
│   ├── pingsweep.png
│   ├── port_scan.png
│   ├── drupal_login.png
│   ├── reverse_shell.png
│   ├── root_flag.png
│   └── ...
│
├── exploits/                 # Optional: scripts used
│   └── php-reverse-shell-modified.php
│
└── credentials.txt           # Optional: credentials discovered (be careful)


# DC-7 VulnHub Walkthrough

**Target**: [DC-7](https://www.vulnhub.com/entry/dc-7,356/)  
**Goal**: Capture the flag and gain root privileges.  
**Techniques**: Reconnaissance, service enumeration, credential harvesting, PHP filter exploitation, reverse shells, cron job abuse.

---

## Summary

DC-7 is a medium-difficulty VulnHub machine that simulates a real-world penetration test. It requires out-of-the-box thinking, leveraging public source code repositories, and abusing misconfigured cron jobs to escalate privileges.

---

## Reconnaissance

- Network scan revealed target at `10.0.2.27`
- Open ports: `22 (SSH)`, `80 (HTTP)`
- Web service running Drupal 8

![Ping sweep](screenshots/pingsweep.png)

---

## Credential Discovery

- Found `@DC7USER` on webpage → GitHub repo → `config.php` with credentials:
  - Username: `dc7user`
  - Password: `MdR3xOgB7#dW`

---

## Initial Access

- SSH login as `dc7user` with discovered credentials
- Used `drush` to reset Drupal admin password: `admin123`
- Enabled PHP Filter module
- Uploaded PHP reverse shell → www-data shell

---

## Privilege Escalation

- Found `/opt/scripts/backup.sh` writable by `www-data` group
- Cron runs this script as root
- Appended reverse shell payload to `backup.sh`
- Received root shell → captured flag

![Root flag](screenshots/root_flag.png)

---

## Flags & Findings

| Flag | Location |
|------|----------|
| Root flag | `/root/flag.txt` |

---

## Tools Used

- Kali Linux
- `ifconfig`, `ping`, `netcat`
- `dirb`, `ssh`, `drush`
- Firefox, Google search
- PHP reverse shell

---

## Lessons Learned

- Always check public source code repositories for credentials.
- `drush` can be a powerful privilege escalation vector.
- Writable cron scripts = almost guaranteed root.

---

## Repository Structure
├── README.md
├── findings.md
├── screenshots/
└── exploits/
