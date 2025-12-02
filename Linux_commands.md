# faillock user check korar jonno
sudo faillock --user {username}

#
sudo -k

ss -tuln | nl     # কোন পোর্ট ওপেন
netstat -tulnp | grep LISTEN   # কোন প্রসেস শুনছে (অথবা ss -tulnp)
systemctl list-unit-files --type=service | grep enabled   # কী কী সার্ভিস অটোস্টার্ট
