sudo passwd -S {username}

 Meaning:
L = Locked 🔒
P = Password set (active) ✅
NP = No password

User lock (immediate disable)
sudo usermod -L shazzad

Account Expire
sudo chage -E 0 username

To Stop Login Shell 
sudo usermod -s /usr/sbin/nologin username

Account Expiry Check
sudo chage -l username

Login shell Check
sudo grep username /etc/shadow
