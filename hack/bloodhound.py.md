bloodhound.py` は、**Active Directory 環境の情報を収集するツールでBloodHoundに対応したデータを出力します。内部的には LDAP を使ってユーザー、グループ、コンピュータ、権限などを列挙します。


python3 bloodhound.py -u <ユーザー名> -p '<パスワード>' -dc <DCホスト名> -d <ドメイン名> -c All

python3 bloodhound.py -u alice -p 'P@ssw0rd!' -dc DC01.fluffy.htb -d fluffy.htb -c All 具体例

bloodhound-python -u judith.mader -p 'judith09' -dc DC01.certified.htb -d certified.htb -c All -ns 10.129.231.186 --dns-tcp

| オプション    | 説明                                                                      |
| -------- | ----------------------------------------------------------------------- |
| `-u`     | ユーザー名（例：alice）                                                          |
| `-p`     | パスワード（クォート推奨）                                                           |
| `-dc`    | ドメインコントローラのホスト名またはIP（例：DC01.fluffy.htb）                                 |
| `-d`     | ドメイン名（例：fluffy.htb）                                                     |
| `-c All` | 収集対象（`All`, `Group`, `User`, `Computer`, `Session`, `Trusts`, `ACL` など） |
結果はカレントディレクトリに出力


| モード       | 説明                              |
| --------- | ------------------------------- |
| `All`     | すべてを収集（デフォルト推奨）                 |
| `User`    | ユーザー情報のみ                        |
| `Group`   | グループ情報のみ                        |
| `Session` | セッション（ログオン）情報（重要）               |
| `ACL`     | ACL権限（Privilege escalation に有用） |
python3 bloodhound.py -hashes <LMHASH>:<NTHASH> -u alice -d fluffy.htb -dc DC01.fluffy.htb -c All  NTLMハッシュ認証

export KRB5CCNAME=/tmp/krb5cc_0
python3 bloodhound.py -k -no-pass -u alice -d fluffy.htb -dc DC01.fluffy.htb -c All
Kerberosチケット認証（例：TGT取得済み）

┌──(kali㉿kali)-[~/HTB/voleur]
└─$ sudo nano /etc/resolv.conf


bloodhound-python -u sqlsvc -p 'Tryhackme!' \
  -d naka.local \
  -dc WIN-CCHP7FUM2S6.naka.local \
  -ns 192.168.3.100 --dns-tcp \
  -c All --disable-autogc


[sudo] password for kali: 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB/voleur]
└─$ dig dc.voleur.htb @10.10.11.76


; <<>> DiG 9.20.11-4+b1-Debian <<>> dc.voleur.htb @10.10.11.76
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 53422
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;dc.voleur.htb.                 IN      A

;; ANSWER SECTION:
dc.voleur.htb.          3600    IN      A       10.10.11.76

;; Query time: 204 msec
;; SERVER: 10.10.11.76#53(10.10.11.76) (UDP)
;; WHEN: Thu Oct 16 14:53:51 UTC 2025
;; MSG SIZE  rcvd: 58


┌──(kali㉿kali)-[~/HTB/voleur]
└─$ bloodhound-python -u ryan.naylor -k -no-pass \
-dc dc.voleur.htb -d voleur.htb \
-c All --disable-autogc --dns-tcp -ns 10.10.11.76


└─$ bloodhound-python -h                                                                     
usage: bloodhound-python [-h] [-c COLLECTIONMETHOD] [-d DOMAIN] [-v] [-u USERNAME] [-p PASSWORD] [-k] [--hashes HASHES] [-no-pass] [-aesKey hex key] [--auth-method {auto,ntlm,kerberos}] [-ns NAMESERVER] [--dns-tcp]
                         [--dns-timeout DNS_TIMEOUT] [-dc HOST] [-gc HOST] [-w WORKERS] [--exclude-dcs] [--disable-pooling] [--disable-autogc] [--zip] [--computerfile COMPUTERFILE] [--cachefile CACHEFILE] [--ldap-channel-binding]
                         [--use-ldaps] [-op PREFIX_NAME]

Python based ingestor for BloodHound LEGACY
For help or reporting issues, visit https://github.com/dirkjanm/BloodHound.py

options:
  -h, --help            show this help message and exit
  -c, --collectionmethod COLLECTIONMETHOD
                        Which information to collect. Supported: Group, LocalAdmin, Session, Trusts, Default (all previous), DCOnly (no computer connections), DCOM, RDP,PSRemote, LoggedOn, Container, ObjectProps, ACL, All (all except
                        LoggedOn). You can specify more than one by separating them with a comma. (default: Default)
  -d, --domain DOMAIN   Domain to query.
  -v                    Enable verbose output

authentication options:
  Specify one or more authentication options. 
  By default Kerberos authentication is used and NTLM is used as fallback. 
  Kerberos tickets are automatically requested if a password or hashes are specified.

  -u, --username USERNAME
                        Username. Format: username[@domain]; If the domain is unspecified, the current domain is used.
  -p, --password PASSWORD
                        Password
  -k, --kerberos        Use kerberos ccache file
  --hashes HASHES       LM:NLTM hashes
  -no-pass              don't ask for password (useful for -k)
  -aesKey hex key       AES key to use for Kerberos Authentication (128 or 256 bits)
  --auth-method {auto,ntlm,kerberos}
                        Authentication methods. Force Kerberos or NTLM only or use auto for Kerberos with NTLM fallback

collection options:
  -ns, --nameserver NAMESERVER
                        Alternative name server to use for queries
  --dns-tcp             Use TCP instead of UDP for DNS queries
  --dns-timeout DNS_TIMEOUT
                        DNS query timeout in seconds (default: 3)
  -dc, --domain-controller HOST
                        Override which DC to query (hostname)
  -gc, --global-catalog HOST
                        Override which GC to query (hostname)
  -w, --workers WORKERS
                        Number of workers for computer enumeration (default: 10)
  --exclude-dcs         Skip DCs during computer enumeration
  --disable-pooling     Don't use subprocesses for ACL parsing (only for debugging purposes)
  --disable-autogc      Don't automatically select a Global Catalog (use only if it gives errors)
  --zip                 Compress the JSON output files into a zip archive
  --computerfile COMPUTERFILE
                        File containing computer FQDNs to use as allowlist for any computer based methods
  --cachefile CACHEFILE
                        Cache file (experimental)
  --ldap-channel-binding
                        Use LDAP Channel Binding (will force ldaps protocol to be used)
  --use-ldaps           Use LDAP over TLS on port 636 by default
  -op, --outputprefix PREFIX_NAME
                        String to prepend to output file names
