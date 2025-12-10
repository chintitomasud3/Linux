লিনুক্স হার্ডেনিং করার আগে মেশিনের বর্তমান অবস্থার (baseline) ডকুমেন্টেশন রাখা খুবই গুরুত্বপূর্ণ। এটা পরে অডিট, রোলব্যাক, কমপ্লায়েন্স চেক বা ট্রাবলশুটিং-এ কাজে লাগে। তোমার স্যার যেটা বলছেন সেটা ঠিকই আছে।

### কী কী ডকুমেন্ট করতে হবে (প্রি-হার্ডেনিং baseline)
নিচের জিনিসগুলো সব সংগ্রহ করে একটা ফোল্ডারে বা Git repo-তে রেখে দাও। তারিখ-টাইমস্ট্যাম্প দিয়ে ফাইলের নাম দিস যাতে বোঝা যায় কবে নেওয়া।

#### ১. হার্ডওয়্যার ও সিস্টেম ইনফরমেশন
```bash
hostnamectl
uname -a
cat /etc/os-release
dmidecode | grep -i "manufacturer\|product\|serial" > hardware_info.txt
lscpu > cpu_info.txt
lsblk -f > disk_info.txt
free -h > memory_info.txt
```

#### ২. ইউজার ও গ্রুপ
```bash
cat /etc/passwd > users_before.txt
cat /etc/shadow > shadow_before.txt   # (খুব সেনসিটিভ, শুধু root দিয়ে রাখো)
cat /etc/group > groups_before.txt
getent passwd | awk -F: '$3 >= 1000 && $3 < 60000' > normal_users.txt
```

#### ৩. সুডো কনফিগ
```bash
cp /etc/sudoers sudoers_before.txt
visudo -c  # চেক করো ভ্যালিড কিনা
ls -l /etc/sudoers.d/ 
cat /etc/sudoers.d/* > sudoers_d_before.txt
```

#### ৪. নেটওয়ার্ক কনফিগ
```bash
ip addr show > network_interfaces.txt
ip route show > routing_table.txt
ss -tuln > listening_ports.txt
firewall-cmd --list-all > firewalld_before.txt   # যদি firewalld থাকে
iptables-save > iptables_before.txt
ufw status verbose > ufw_before.txt   # যদি ufw থাকে
cat /etc/hosts > hosts_file.txt
```

#### ৫. ইনস্টলড প্যাকেজ লিস্ট
```bash
# Debian/Ubuntu
dpkg --get-selections > installed_packages_debian.txt
dpkg -l | grep ^ii > packages_list.txt

# RHEL/CentOS/Rocky/Alma
rpm -qa --qf "%{NAME}-%{VERSION}-%{RELEASE}.%{ARCH}\n" | sort > installed_packages_rpm.txt
yum list installed > yum_packages.txt  # বা dnf
```

#### ৬. রানিং সার্ভিস
```bash
systemctl list-unit-files --type=service > services_unit_files.txt
systemctl list-units --type=service --state=running > running_services.txt
ps auxf > process_tree.txt
```

#### ৭. কার্নেল প্যারামিটার ও সিকিউরিটি সেটিংস
```bash
sysctl -a > sysctl_all_before.txt
cat /etc/sysctl.conf > sysctl.conf_before
cat /etc/security/limits.conf > limits.conf_before
cat /etc/pam.d/* > pam_configs/    # একটা ফোল্ডারে কপি করো
```

#### ৮. ফাইল সিস্টেম পারমিশন (গুরুত্বপূর্ণ ফাইল)
```bash
ls -la /etc/passwd /etc/shadow /etc/ssh/sshd_config /etc/fstab > critical_files_perm.txt
find /etc -type f -perm /6000 -ls > suid_sgid_files.txt   # SUID/SGID ফাইল
stat /etc/passwd /etc/shadow /root > important_stats.txt
```

#### ৯. SSH কনফিগ
```bash
cp /etc/ssh/sshd_config sshd_config_before.txt
cp /etc/ssh/sshd_config.d/* sshd_config.d_before/   # যদি থাকে
```

#### ১০. SELinux / AppArmor (যদি থাকে)
```bash
sestatus -v > selinux_status.txt
getenforce > selinux_mode.txt
aa-status > apparmor_status.txt   # Ubuntu তে
```

#### ১১. ক্রন জব ও শিডিউলড টাস্ক
```bash
crontab -l > root_crontab.txt   # root হিসেবে
for user in $(cut -f1 -d: /etc/passwd); do crontab -u $user -l; done > all_crontabs.txt 2>/dev/null
ls -l /etc/cron.* > cron_dirs.txt
```

#### ১২. লগিং কনফিগ
```bash
cat /etc/rsyslog.conf > rsyslog.conf_before
cat /etc/systemd/journald.conf > journald.conf_before
```

### কিভাবে সব এক জায়গায় রাখবে (সাজেশন)
একটা ফোল্ডার বানাও, নাম দাও:
```bash
mkdir -p ~/hardening_baseline/$(hostname)_$(date +%Y%m%d_%H%M)
cd ~/hardening_baseline/$(hostname)_$(date +%Y%m%d_%H%M)
```
তারপর উপরের সব কমান্ড রান করে ফাইলগুলো এখানে সেভ করো। শেষে zip বা tar করে রাখো:
```bash
tar -czf ../$(hostname)_baseline_$(date +%Y%m%d).tar.gz .
```
Git repo-তে পুশ করতে পারো (private repo) অথবা USB-এ রাখো।

### হার্ডেনিং-এর পর আবার একই ডকুমেন্টেশন নিবি
তখন নাম দিবি `after` দিয়ে। তুলনা করতে পারবি কী কী চেঞ্জ হয়েছে।

এই baseline থাকলে তোমার স্যার খুব খুশি হবে, আর পরে কোনো সমস্যা হলে রোলব্যাক করা সহজ হবে।

যদি কোনো নির্দিষ্ট ডিস্ট্রো (Ubuntu/CentOS/Rocky) বলো, আমি আরো স্পেসিফিক স্ক্রিপ্ট দিতে পারি। শুভকামনা!


# ১. সব world-writable ফাইল (sticky bit ছাড়া)
find / -type f -perm -o+w ! -perm -1000 2>/dev/null

# ২. সব world-writable ডিরেক্টরি (sticky bit ছাড়া) ← সবচেয়ে বিপজ্জনক
find / -type d -perm -o+w ! -perm -1000 2>/dev/null

# ৩. সাধারণ বিপজ্জনক জায়গা চেক
ls -la /tmp /var/tmp /dev/shm
df -h | grep -E "(tmp|shm)"

# ৪. গুরুত্বপূর্ণ ফাইল world-writable কিনা
ls -la /etc/passwd /etc/shadow /etc/crontab /etc/cron.d/ /root/.ssh/authorized_keys
