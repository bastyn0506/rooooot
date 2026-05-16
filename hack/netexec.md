netexec smb 10.129.243.199 -u WEB_SVC -p 'dksehdgh712!@#' --shares

netexec smb ip dc確認できる

netexec smb ip --generate-krb5-file name.krb kerberosfile作成

netexec プロトコル　ip -u user -p pass 確認　NTLM false ntlmサポートされてない　-kでkerberos使えば行けるときあり

smb --shares ファイル共有確認

-M timeroast 

netexec smb 10.129.230.181 -u 'a' -p '' --shares

netexec ldap 10.129.96.155 -u '' -p '' --users

netexec smb 10.129.96.155 -u u.txt -p 'Welcom123!' -d megabank.local --continue-on-success