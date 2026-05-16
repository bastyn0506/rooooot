
Basic System Information
È Check if the Windows versions is vulnerable to some known exploit https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#version-exploits
winPEASany_ofs.exe : ERROR: Access denied
    + CategoryInfo          : NotSpecified: (ERROR: Access denied:String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
  [X] Exception: Access denied 
  [X] Exception: Access denied 
  [X] Exception: The given key was not present in the dictionary.

### ここが意味していること

- winPEASは
    
    - OSバージョンから「既知のLPEエクスプロイトがあるか」
        
    - インストールされている更新プログラム（パッチ状況）  
        をチェックしようとしているけど、**今の権限だと読めない情報がある**ので `Access denied` になっている。
        
- HTBの現実的な意味としては：
    
    - 「OSの既知LPE(CVE-xxxx)の自動サジェスト」は使えない状態。
        
    - でもこれは**よくあることで、致命的ではない**。  
        → 別ルート（サービス設定・権限ミス・SQL・AlwaysInstallElevatedなど）を狙えばOK。


User Environment Variables ユーザー環境変数
È Check for some passwords or keys in the env variables 
    COMPUTERNAME: DC01
    PUBLIC: C:\Users\Public
    LOCALAPPDATA: C:\Users\adam.scott\AppData\Local
    PSModulePath: C:\Users\adam.scott\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShell\Modules;C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules;C:\Program Files (x86)\Microsoft SQL Server\160\Tools\PowerShell\Modules\
    PROCESSOR_ARCHITECTURE: AMD64
    Path: C:\Program Files\Python312\Scripts\;C:\Program Files\Python312\;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\WINDOWS\System32\WindowsPowerShell\v1.0\;C:\WINDOWS\System32\OpenSSH\;C:\Program Files (x86)\Microsoft SQL Server\160\Tools\Binn\;C:\Program Files\Microsoft SQL Server\160\Tools\Binn\;C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\170\Tools\Binn\;C:\Program Files\Microsoft SQL Server\160\DTS\Binn\;C:\Users\adam.scott\AppData\Local\Microsoft\WindowsApps
    CommonProgramFiles(x86): C:\Program Files (x86)\Common Files
    ProgramFiles(x86): C:\Program Files (x86)
    PROCESSOR_LEVEL: 25
    ProgramFiles: C:\Program Files
    PATHEXT: .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC;.PY;.PYW;.CPL
    USERPROFILE: C:\Users\adam.scott
    SystemRoot: C:\WINDOWS
    ALLUSERSPROFILE: C:\ProgramData
    DriverData: C:\Windows\System32\Drivers\DriverData
    ProgramData: C:\ProgramData
    PROCESSOR_REVISION: 0101
    USERNAME: adam.scott
    CommonProgramW6432: C:\Program Files\Common Files
    CommonProgramFiles: C:\Program Files\Common Files
    OS: Windows_NT
    PROCESSOR_IDENTIFIER: AMD64 Family 25 Model 1 Stepping 1, AuthenticAMD
    ComSpec: C:\WINDOWS\system32\cmd.exe
    SystemDrive: C:
    TEMP: C:\Users\ADAM~1.SCO\AppData\Local\Temp
    NUMBER_OF_PROCESSORS: 2
    APPDATA: C:\Users\adam.scott\AppData\Roaming
    TMP: C:\Users\ADAM~1.SCO\AppData\Local\Temp
    ProgramW6432: C:\Program Files
    windir: C:\WINDOWS
    USERDOMAIN: EIGHTEEN
    USERDNSDOMAIN: eighteen.htb


### 1. COMPUTERNAME / USERDOMAIN / USERDNSDOMAIN

    `COMPUTERNAME: DC01     USERDOMAIN: EIGHTEEN     USERDNSDOMAIN: eighteen.htb`

- **COMPUTERNAME: DC01**
    
    - このマシンは `DC01` → **ドメインコントローラ**。
        
    - つまり今触ってるのはただのメンバーサーバーじゃなくて、「ADの心臓部」。
        
- **USERDOMAIN / USERDNSDOMAIN**
    
    - ドメイン名 `EIGHTEEN` / FQDN `eighteen.htb`。
        
    - HTBマシン名とも一致してるね。
        

> ★ 重要ポイント：  
> **「DC上のドメインユーザーでシェルを持っている」**というだけで、  
> うまくいけば**ドメイン管理者レベルまで行ける可能性がある**。  
> この後の BloodHound / AD系攻撃がかなり有効になるパターン。

---

### 2. USERNAME / USERPROFILE / LOCALAPPDATA / APPDATA

    `USERNAME: adam.scott     USERPROFILE: C:\Users\adam.scott     LOCALAPPDATA: C:\Users\adam.scott\AppData\Local     APPDATA: C:\Users\adam.scott\AppData\Roaming`

- 現在のコンテキストは
    
    - **ドメインユーザー `EIGHTEEN\adam.scott`**
        
- `C:\Users\adam.scott` 配下は**基本的に書き込み可能**なので：
    
    - `Downloads` に自分のツールを落とす
        
    - `.ssh`, `.rdp`, `AppData\Roaming` に保存された認証情報を探す
        
    - ブラウザのクレデンシャル・RDPキャッシュなどを後で狙う  
        といった「ユーザープロファイルからの横展開ネタ」候補になる。
        

> ★ 後でやるべき動き：
> 
> - `C:\Users\adam.scott\Desktop`, `Documents`, `Downloads` を手作業で漁る
>     
> - `AppData\Roaming` 内のアプリ設定（VPN, RDPなど）に資格情報がないかチェック
>     

---

### 3. PSModulePath（PowerShellモジュールパス）

    `PSModulePath:        C:\Users\adam.scott\Documents\WindowsPowerShell\Modules;       C:\Program Files\WindowsPowerShell\Modules;       C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules;       C:\Program Files (x86)\Microsoft SQL Server\160\Tools\PowerShell\Modules\`

ここ重要。

- 最初のパス：
    
    `C:\Users\adam.scott\Documents\WindowsPowerShell\Modules`
    
    - **ユーザー書き込み可能な PowerShell モジュールディレクトリ**。
        
    - もし管理者がタスクやログオンスクリプトで  
        `Import-Module 何か` をしていて、それがここを見に来るなら、  
        **モジュールハイジャック（悪意ある ps1 を置き換え）→UAC/権限昇格**の足掛かりになる。
        
    
    ただし：
    
    - winPEASだけでは「どのモジュールがどう使われてるか」までは分からない。
        
    - 後で `Scheduled Tasks`, `Logon scripts`, `Profile.ps1` と組み合わせて見ていく感じ。
        
- SQL Server関連のモジュールパス：
    
    `C:\Program Files (x86)\Microsoft SQL Server\160\Tools\PowerShell\Modules\`
    
    - **SQL Server 2022 っぽいものが入っている**のが分かる。
        
    - これは後で出てくるであろう「MSSQLサービス」や「ポート1433」と絡んで、
        
        - `xp_cmdshell`
            
        - `impacket-mssqlclient.py`
            
        - PowerShell の `Invoke-Sqlcmd`  
            などから OS コマンド実行 → 権限昇格ルートになりやすい。
            

> ★ 今の時点でのメモ：
> 
> - **「MSSQL入ってるな」**という事実はめちゃ大事。  
>     → 後で `winPEAS` の `Services` / `Listening Ports` / `Installed Programs` でも確認したいポイント。
>     
> - PowerShellモジュールハイジャックは、
>     
>     - 「自分が管理者セッションを取れてるのに、UACだけ回避したい」
>         
>     - 「特定のスクリプトが自動実行される」  
>         みたいな前提が必要なので、今は頭の片隅に置くくらいでOK。
>         

---

### 4. PATH / PATHEXT / Python / SQL Server

    `Path: C:\Program Files\Python312\Scripts\;C:\Program Files\Python312\; ...;           C:\Program Files (x86)\Microsoft SQL Server\160\Tools\Binn\; ...`

- Python3.12 がインストールされていて、**システムの PATH に通っている**。
    
- SQL Server の `Binn` も PATH に入っている。
    

これ自体は「弱設定」ではないけど、**他のミス設定と組み合わさると攻撃材料**になる：

- PATH汚染系：
    
    - もし「書き込み可能なディレクトリ」が PATH の中に紛れていたら、  
        そこに `schtasks.exe` や `whoami.exe` などの偽バイナリを置いて**コマンドハイジャック**できる。
        
    - 今の一覧を見る限り、PATHに含まれているのは基本的に `C:\Windows` / `Program Files` 系だけで、  
        **ユーザー書き込み可能パスは含まれてなさそう**。  
        → つまり、**今見えてる範囲では PATH 経由の簡単なハイジャックは難しそう**。
        
- MSSQL:
    
    - SQLコマンドラインツールを叩きやすい環境。
        
    - 後で「SQLログイン情報が手に入った」「アプリ設定ファイルからSQL接続文字列が出た」  
        となったときに、すぐに使える。
        

---

### 5. その他の環境変数

残りは基本的に「普通のWindows環境変数」で、直接のLPEネタになりにくい：

    `TEMP / TMP: C:\Users\ADAM~1.SCO\AppData\Local\Temp     ProgramFiles, ProgramFiles(x86), CommonProgramFiles...     SystemRoot: C:\WINDOWS     windir: C:\WINDOWS     NUMBER_OF_PROCESSORS: 2     OS: Windows_NT     PROCESSOR_* ...`

- `TEMP` / `TMP` はこのユーザーの一時ディレクトリ。  
    一時ファイルを経由する攻撃（DLLインジェクション、インストーラ系）の時に意識する場所。
    
- CPUコア数などはほぼ情報的意味のみ。
    

> ★ チェックポイント：
> 
> - 「環境変数に平文パスワードが埋まってないか？」を見るところだけど、  
>     今回は**パスワードらしき文字列は無し**。





