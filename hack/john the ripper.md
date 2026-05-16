- **目的**：ハッシュ化されたパスワードの解析（総当たり / 辞書攻撃など）
    
- **開発元**：Openwall（https://www.openwall.com/john/）
    
- **対応フォーマット**：LM, NTLM, bcrypt, md5crypt, sha512crypt, ZIP, RAR, SSH, Kerberos、etc.
    
- **高速版**：`john`（CPU）、`john --format=... --fork=N`（マルチプロセス）  
    GPUなら `hashcat` が主流だが、JtRも強力

| シナリオ                | やること                                  |
| ------------------- | ------------------------------------- |
| ADからNTLMハッシュを取得     | `secretsdump.py` → `john --format=nt` |
| SSH秘密鍵の解析           | `ssh2john` → `john`                   |
| ZIPファイルのパスワード解析     | `zip2john` → `john`                   |
| Linuxの/etc/shadow解析 | `unshadow` → `john`                   |

john <ハッシュファイル> --wordlist=<辞書ファイル>
john hashes.txt --wordlist=/usr/share/wordlists/rockyou.txt 例

john --show <ハッシュファイル>### クラック成功パスワードの確認

john --session=myjob hashes.txt --wordlist=rockyou.txt　保存
john --restore=myjob　再開

john --format=nt --wordlist=rockyou.txt hashes.txt
形式例：

- `raw-md5`
    
- `raw-sha1`
    
- `bcrypt`
    
- `nt`（Windows NTLMハッシュ）
    
- `zip`, `rar`, `ssh`, `krb5tgs`（Kerberos）
john --list=formats　対応フォーマット表示

## 💡 Tips

- `john --incremental` → 総当たりモード（非常に時間がかかる）
    
- `john --rules` → パスワード変種（数字付き、反転など）を自動で試す
    
- クラッキング速度を上げるには CPU スレッド数指定や GPU ツール（hashcat）も検討



┌──(kali㉿kali)-[/usr/share/wordlists]
└─$ ls
amass  dirb  dirbuster  dnsmap.txt  fern-wifi  john.lst  metasploit  nmap.lst  rockyou.txt  seclists  sqlmap.txt  wfuzz  wifite.txt







