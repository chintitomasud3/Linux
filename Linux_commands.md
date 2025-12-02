# faillock user check korar jonno
sudo faillock --user {username}

#
sudo -k
# কোন পোর্ট ওপেন
ss -tuln | nl
# কোন প্রসেস শুনছে (অথবা ss -tulnp)
netstat -tulnp | grep LISTEN   
systemctl list-unit-files --type=service | grep enabled   # কী কী সার্ভিস অটোস্টার্ট

