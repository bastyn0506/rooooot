## Gobusterとは？

- **HTTPディレクトリ列挙**、**DNSサブドメイン列挙**、**VHOST列挙** などに使えるCLIツール
    
- Goで書かれており非常に高速

gobuster dir -u http://TARGET -w WORDLIST

gobuster dir -u http://10.10.10.100 -w /usr/share/wordlists/dirb/common.txt

gobuster dir -u http://10.10.10.100 -w common.txt -x php,html,txt 拡張子指定

gobuster dir -u http://10.10.10.100 -w common.txt -b 403　ステータスコード除外

gobuster dir -u http://10.10.10.100 -w common.txt -b 403　DNS サブドメイン列挙

gobuster dir -u http://10.10.11.58 -w /usr/share/wordlists/dirb/common.txt

┌──(kali㉿kali)-[/usr/share/seclists/Discovery/Web-Content]
└─$ gobuster dir -u http://10.10.11.72 -w common.txt


gobuster dir -u http://eighteen.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,zip,bak -t 30

gobuster dir -u http://monitorsfour.htb -w /usr/share/wordlists/dirb/common.txt


| オプション | 意味                            |
| ----- | ----------------------------- |
| `-u`  | URL                           |
| `-w`  | ワードリスト                        |
| `-x`  | 拡張子（例: `php,html`）            |
| `-t`  | 並列スレッド数（例: `-t 50`）           |
| `-b`  | 除外するステータスコード（例: `-b 404,403`） |
| `-s`  | マッチするステータスコード                 |
| `-k`  | 証明書無視（HTTPSでもOK）              |
| `-r`  | リダイレクトを追わない                   |
| `-o`  | 出力ファイル保存                      |


|         |                |                        |
| ------- | -------------- | ---------------------- |
| `dir`   | ディレクトリ列挙       | `/admin`, `/login` など  |
| `dns`   | サブドメイン列挙       | `admin.example.com` など |
| `vhost` | バーチャルホスト列挙     | `foo.target.com` など    |
| `s3`    | AWS S3バケット列挙   |                        |
| `fuzz`  | 汎用的なFuzz（v3以降） |                        |
