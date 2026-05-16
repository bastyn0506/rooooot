nmap

[[netexec]]  [[dig]] [[smbcliant]] [[mono]]

[[bloodhound]]


- 権限確認
    
    `whoami /priv whoami /groups`
    
- サービス確認
    
    `wmic service get name,pathname,startmode`
    
- 開放ポート確認
    
    `netstat -ano`
    
- グループ・ユーザ確認
    
    `net user net localgroup administrators`