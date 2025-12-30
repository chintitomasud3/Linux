Linux hardening ekta comprehensive process, jekhane tumi system ke secure korte paro unauthorized access, exploits ar vulnerabilities theke. Ekhane step-by-step guide dilam:

## 1. **System Update & Patch Management**
```bash
# Full system update
sudo apt update && sudo apt upgrade -y  # Debian/Ubuntu
sudo dnf update -y                     # Fedora/RHEL
sudo pacman -Syu                       # Arch

# Auto security updates
sudo apt install unattended-upgrades
sudo dnf install dnf-automatic
```

## 2. **User & Authentication Hardening**
```bash
# Root login disable
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo systemctl restart sshd

# Strong password policy
sudo sed -i 's/PassMinLength 8/PassMinLength 12/' /etc/security/pwquality.conf

# Fail2ban install
sudo apt install fail2ban
```

## 3. **SSH Hardening**
```bash
# /etc/ssh/sshd_config edit koro:
PermitRootLogin no
PasswordAuthentication no  # Key-based only
PubkeyAuthentication yes
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 0

# Restart SSH
sudo systemctl restart sshd
```

## 4. **Firewall Configuration**
```bash
# UFW (Ubuntu)
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable

# firewalld (RHEL/Fedora)
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

## 5. **SELinux/AppArmor Enable**
```bash
# SELinux (RHEL/CentOS)
sudo setenforce 1
sudo sed -i 's/SELINUX=permissive/SELINUX=enforcing/' /etc/selinux/config

# AppArmor (Ubuntu)
sudo aa-enforce /etc/apparmor.d/*
```

## 6. **Kernel Hardening**
```bash
# sysctl.conf (/etc/sysctl.conf or /etc/sysctl.d/99-hardening.conf)
net.ipv4.ip_forward = 0
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
kernel.kptr_restrict = 2
kernel.dmesg_restrict = 1
kernel.randomize_va_space = 2
vm.swappiness = 10

# Apply
sudo sysctl -p
```

## 7. **File Permissions & Ownership**
```bash
sudo chmod 600 /etc/shadow
sudo chmod 644 /etc/passwd
sudo chmod 600 ~/.ssh/authorized_keys
sudo chown root: /etc/sudoers
sudo chmod 440 /etc/sudoers
```

## 8. **Disable Unnecessary Services**
```bash
# Check running services
sudo systemctl list-unit-files --type=service --state=enabled

# Disable common unnecessary services
sudo systemctl disable --now avahi-daemon cups bluetooth
```

## 9. **Install Security Tools**
```bash
# Intrusion Detection
sudo apt install aide rkhunter chkrootkit lynis

# Log monitoring
sudo apt install auditd rsyslog

# Initial scans
sudo rkhunter --update && sudo rkhunter --check
sudo aide --init
```

## 10. **Grub & Boot Hardening**
```bash
# /etc/default/grub edit
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash console=tty1 console=ttyS0,115200n8 mitigations=auto"

sudo update-grub
```

## 11. **Automated Hardening Scripts**
```bash
# CIS Benchmark scripts
wget https://github.com/ansible-lockdown/ansible-linux-landscape/raw/master/roles/linux-landscape/tasks/main.yml

# Lynis audit
sudo lynis audit system
```

## 12. **Monitoring & Logging**
```bash
# Log rotation ar central logging setup
sudo apt install logrotate rsyslog-gnutls

# Auditd rules
echo 'auditctl -w /etc/passwd -p wa -k identity' | sudo tee -a /etc/audit/rules.d/audit.rules
sudo systemctl restart auditd
```

## Quick Checklist:
```
☐ System fully updated
☐ SSH key-only auth, root disabled  
☐ Firewall enabled + SSH only
☐ SELinux/AppArmor enforcing
☐ Unnecessary services disabled
☐ Strong password policies
☐ Fail2ban running
☐ AIDE baseline created
☐ Kernel hardened
☐ Regular Lynis/Chkrootkit scans
```

**Pro Tip**: `ansible` use koro multiple systems er jonno, ba `linux-hardening` Docker container diye test koro first.

Konta specifically implement korte chai? Ami detailed script dibo! 🚀
