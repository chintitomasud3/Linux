Here are some essential Linux administration commands organized by category:

---

## 🖥️ System Information
```bash
uname -a          # Full system info (kernel, hostname, OS)
hostnamectl       # Hostname and OS details
uptime            # How long system has been running
lscpu             # CPU architecture info
lsmem             # Memory info
dmidecode         # Hardware details (run as root)
```

---

## 👤 User Management
```bash
adduser username          # Add a new user
passwd username           # Change user password
usermod -aG sudo username # Add user to sudo group
deluser username          # Delete a user
who                       # Who is logged in
id username               # Show user UID, GID, groups
last                      # Login history
```

---

## 📁 File & Directory
```bash
ls -lah               # List files with size in human-readable format
find / -name "file"   # Search for a file
du -sh *              # Disk usage of each item in current dir
chmod 755 file        # Change file permissions
chown user:group file # Change file ownership
tar -czvf out.tar.gz /dir  # Compress a directory
tar -xzvf file.tar.gz      # Extract archive
```

---

## 💾 Disk & Storage
```bash
df -h               # Disk space usage (human-readable)
lsblk               # List block devices (disks, partitions)
fdisk -l            # Partition details (run as root)
mount /dev/sdb1 /mnt  # Mount a disk
umount /mnt           # Unmount
blkid               # Show UUIDs of partitions
```

---

## 🧠 Memory & CPU
```bash
free -h             # RAM usage
top                 # Real-time process monitor
htop                # Interactive process viewer (install if needed)
vmstat 1            # Memory/CPU stats every 1 second
iostat              # CPU and I/O stats
```

---

## 🔧 Process Management
```bash
ps aux              # List all running processes
kill PID            # Kill process by ID
kill -9 PID         # Force kill
pkill processname   # Kill by name
nice -n 10 command  # Run command with lower priority
nohup command &     # Run command in background, immune to hangups
```

---

## 🌐 Networking
```bash
ip a                     # Show IP addresses
ip r                     # Show routing table
ping google.com          # Test connectivity
netstat -tulnp           # Open ports and listening services
ss -tulnp                # Modern alternative to netstat
curl -I https://site.com # Check HTTP headers
wget URL                 # Download a file
traceroute google.com    # Trace network path
nslookup domain.com      # DNS lookup
```

---

## 🔥 Firewall (UFW / iptables)
```bash
ufw status              # Firewall status
ufw enable              # Enable firewall
ufw allow 22/tcp        # Allow SSH
ufw deny 80             # Block port 80
ufw delete allow 80     # Remove a rule
iptables -L             # List iptables rules
```

---

## ⚙️ Service Management (systemd)
```bash
systemctl status nginx       # Check service status
systemctl start nginx        # Start a service
systemctl stop nginx         # Stop a service
systemctl restart nginx      # Restart a service
systemctl enable nginx       # Enable on boot
systemctl disable nginx      # Disable on boot
journalctl -u nginx -f       # Live logs for a service
```

---

## 📦 Package Management
```bash
# Debian/Ubuntu
apt update && apt upgrade    # Update all packages
apt install package          # Install a package
apt remove package           # Remove a package
dpkg -l                      # List installed packages

# RHEL/CentOS
yum update                   # Update packages
dnf install package          # Install (newer systems)
rpm -qa                      # List installed RPM packages
```

---

## 📋 Logs & Monitoring
```bash
tail -f /var/log/syslog      # Live system log
journalctl -xe               # Systemd logs with errors
dmesg | tail                 # Kernel ring buffer
cat /var/log/auth.log        # Authentication logs
logrotate                    # Manage log rotation
```

---

## 🔐 SSH & Security
```bash
ssh user@host                       # Connect to remote server
ssh-keygen -t rsa -b 4096           # Generate SSH key pair
ssh-copy-id user@host               # Copy public key to server
scp file user@host:/path            # Secure copy file
fail2ban-client status              # Check fail2ban (brute-force protection)
grep "Failed password" /var/log/auth.log  # Check failed logins
```

---

## 🛠️ Useful Shortcuts
```bash
!!              # Repeat last command
history         # Command history
ctrl + r        # Search command history
alias ll='ls -lah'  # Create a shortcut
crontab -e      # Edit cron jobs (scheduled tasks)
watch -n 2 df -h    # Repeat a command every 2 seconds
```

---

> 💡 **Pro Tips:**
> - Always use `man command` (e.g. `man ls`) to read the manual for any command.
> - Use `sudo` before commands that need root privileges.
> - Be careful with `rm -rf` — it deletes files permanently without confirmation.

Would you like a deeper dive into any specific category?
