`**smbmap**` は、Windowsの**SMB（Server Message Block）共有の列挙**に特化したペンテストツールです。共有フォルダのアクセス権や読み書き可能ファイルを調べることができます。

smbmap -H {ip}
smbmap -H -u {username} -p {password}
smbmap -H -u -p -d {domain}
smbmap -H -u -p -R | grep 'txt'
smbmap -H -u -p ' ' -H {hash}

