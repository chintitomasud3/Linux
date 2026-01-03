grep -v -e '^#' -e '^$' filename










sudo apt update
sudo apt install auditd audispd-plugins
systemctl disable cups
systemctl list-units | grep service


netstat -ano | findstr 1801
Get-Service | Where-Object { $_.DisplayName -like "*Message Queuing*" }


Terminal of Sessions
Identify the session
Command : loginctl list-sessions
Terminate the session cleanly
sudo loginctl terminate-session 3


sudo grep ehsan /var/log/secure
sudo journalctl -u sshd
sudo journalctl -u sshd -n 20 |tail -n +2|nl


Audit


# সফল লগইন
sudo ausearch -m USER_LOGIN -sv yes

# ব্যর্থ লগইন
sudo ausearch -m USER_LOGIN -sv no

# ব্যর্থ SSH লগইন
sudo ausearch -m AVC -ts recent | grep sshd



টুল	কমান্ড / ইউজ	কী দেখাবে
netstat / ss	`ss -antp	grep ESTAB`
lsof -i	`lsof -i -P -n	grep ESTAB`
nethogs	sudo nethogs	কোন প্রসেস কত ব্যান্ডউইথ খাচ্ছে
iftop	sudo iftop -i eth0	রিয়েল টাইম ট্রাফিক মনিটর
tcpdump	sudo tcpdump -i any -nn not port 22	প্যাকেট লেভেল ক্যাপচার
wireshark (GUI)	GUI তে ফিল্টার: http.request or tls.handshake	API কল দেখা (JSON, XML)
strace (যদি সন্দেহ হয়)	strace -f -e trace=network -p <PID>	কোন প্রসেস কোন IP তে কানেক্ট করছে
opensnitch (Firewall)	ইন্সটল করে রাখো → সব আউটবাউন্ড ব্লক করে জিজ্ঞাসা করবে	প্রোডাকশনে বেস্ট


for service in cups autofs avahi-daemon cockpit dhcpd named dnsmasq vsftpd pure-ftpd nfs-server rpcbind ybind nisd rsyncd smb nmb snmpd telnet.socket squid httpd nginx xinetd gdm xorg postfix sendmail ypbind tftp; do
  echo -n "$service : "
  systemctl is-enabled $service 2>/dev/null || echo "not found"
done




# 2


find / -xdev -type f -perm -0002 -exec ls -ld {} \; 2>/dev/null 

perm -002 =যেসব ফাইলে"others" (অন্যরা) write permission আছে 0002 = others-writable)



# 3
sudo ss -tulpen | egrep '5353|137|138|139|445|bluetooth'


stat /etc/{passwd,shadow,group,gshadow,sudoers,ssh/sshd_config}
ls -la  /etc/{passwd,shadow,group,gshadow,sudoers,ssh/sshd_config}

#world writable file

find / -xdev -type f -perm -0002 -exec ls -ld {} \; 2>/dev/null

#find folder and file without owner







#awk -F: '{ print $1 ":" $4 }' /etc/passwd | while IFS=: read user gid; do
    if ! getent group "$gid" >/dev/null; then
        echo "User $user has non-existent primary group GID $gid"
    fi
done


# Home directory Permission Check

awk -F: '{print $1 ":" $6}' /etc/passwd | while IFS=: read user home; do
    if [ -d "$home" ]; then
        perms=$(stat -c "%a %U %G" "$home")
        echo "$home ($user) permissions: $perms"
    fi
done



#bash-rc

awk -F: '{print $1 ":" $6}' /etc/passwd | while IFS=: read user home; do
    if [ -d "$home" ]; then
        find "$home" -maxdepth 1 -type f -name ".*" -exec ls -ld {} \; 2>/dev/null
    fi
done











Hardware and system information
hostnamectl
uname -a
cat /etc/os-release
dmidecode | grep -i "manufacturer\|product\|serial" > hardware_info.txt
lscpu > cpu_info.txt
lsblk -f > disk_info.txt
free -h > memory_info.txt

