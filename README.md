*This project has been created as part of the 42 curriculum by tsaby.*

# Born2beroot

## Description

**Born2beroot** is the first system administration project of the 42 curriculum: setting up a secure **Debian** server inside a virtual machine (VirtualBox), following strict rules, and being able to explain every choice during the defense.

This repository contains my revision notes for the defense ([`defense-notes.md`](defense-notes.md), in French). The virtual machine itself is not versioned.

## What was configured

| Area | Configuration |
|---|---|
| Operating system | Debian (stable), no graphical interface |
| Partitions | encrypted partitions with **LVM** |
| Security | **AppArmor** enabled at boot |
| SSH | service on port **4242**, root login disabled |
| Firewall | **UFW**, only port 4242 open |
| Hostname / users | hostname `tsaby42`, user `tsaby` in the `sudo` and `user42` groups |
| Password policy | expiry every 30 days, minimum 2 days between changes, warning 7 days before; at least 10 characters with an uppercase letter, a lowercase letter and a digit, no more than 3 identical consecutive characters, must not contain the user name (`/etc/login.defs`, `libpam-pwquality`) |
| sudo | 3 password attempts, custom error message, every command logged in `/var/log/sudo/`, TTY mode required, restricted secure paths |
| Monitoring | `monitoring.sh` broadcast with `wall` every 10 minutes by `cron` |

### Monitoring script

Every 10 minutes the script displays on all terminals:

- the OS architecture and kernel version;
- the number of physical and virtual processors;
- the RAM and disk usage, and the CPU load;
- the date and time of the last reboot;
- whether LVM is active;
- the number of active TCP connections and logged-in users;
- the server's IPv4 and MAC addresses;
- the number of commands run with `sudo`.

## Useful commands

```bash
sudo ufw status                   # firewall rules
sudo systemctl status ssh         # SSH service
sudo aa-status                    # AppArmor
lsblk                             # partitions and LVM
groups tsaby                      # user groups
sudo chage -l tsaby               # password expiry rules
sudo adduser <user> && sudo usermod -aG user42 <user>
sudo hostnamectl set-hostname <new_hostname>
sudo crontab -e                   # monitoring schedule
```

## Resources

- [Debian administrator's handbook](https://debian-handbook.info/)
- [AppArmor — Debian wiki](https://wiki.debian.org/AppArmor)
- [UFW — Ubuntu documentation](https://help.ubuntu.com/community/UFW)
- `man sudoers`, `man pam_pwquality`, `man login.defs`, `man crontab`
