[[bloodhound.py]]
[[sharphound]]


pass Nakahara02160506@

**BloodHound** は、Active Directory (AD) 環境における **権限関係や攻撃経路を可視化するツール** です。  
内部的には **グラフ理論**を用いて、以下のような問いに答えます：

- 誰がドメイン管理者になれるか？
    
- 誰がどのマシンにログオンしているか？
    
- 特定ユーザーが持っている「間接的な権限」は？


| 攻撃内容                        | 概要                                   |
| --------------------------- | ------------------------------------ |
| **Kerberoasting**           | SPNを持つユーザーのTGSチケットを取得し、オフラインでパスワード解析 |
| **ACL-based escalation**    | ユーザーが他のオブジェクトに書き込み権限を持つ              |
| **RDP/Session情報からの横展開**     | ログオン中ユーザーから権限昇格を狙う                   |
| **Group Delegation Attack** | GenericAll/WriteDACL権限などから昇格可能       |
| 環境                          | 方法                                   |
| Windows                     | `SharpHound.exe`（GUIやコマンドラインで実行）     |
| Linux                       | `bloodhound.py`（Impacketに同梱）         |
| 方法                          | 説明                                   |
| ユーザー・パスワード                  | 一般的なADユーザーでOK（要ネットワーク到達性）            |
| NTLMハッシュ                    | Pass-the-Hash（`-hashes` オプション）       |
| Kerberosチケット                | TGT取得後に `-k -no-pass` で実行可能          |


| エッジ               | 意味                    |
| ----------------- | --------------------- |
| 🔁 `MemberOf`     | ユーザーがグループに所属          |
| 🔓 `AdminTo`      | ユーザーがPC/グループに管理者権限あり  |
| ✍️ `GenericWrite` | ユーザーが対象に書き込み権限あり（重要）  |
| 🔐 `CanRDP`       | RDPログイン可能             |
| 📡 `HasSession`   | 誰かがログオン中のマシン（横展開チャンス） |


| クエリ名                                     | 説明                                                      |
| ---------------------------------------- | ------------------------------------------------------- |
| **Shortest Paths to High Value Targets** | 自分のユーザーからドメイン管理者や高権限ユーザーへの最短経路を表示                       |
| **Shortest Paths to Domain Admins**      | Domain Admin までの最短パス                                    |
| **Users with Admin Rights on Computers** | 管理者権限を持つユーザー一覧                                          |
| **Kerberoastable Users**                 | SPN（Service Principal Name）付きのユーザーを列挙（Kerberoasting 対象） |
| **AS-REP Roastable Users**               | Pre-auth 不要ユーザーを列挙（AS-REP Roasting 対象）                  |
| **Users with Foreign Group Membership**  | 他ドメインのグループに所属しているユーザー                                   |
| **Users with Unconstrained Delegation**  | Unconstrained Delegation を持つユーザー（チケット奪取の可能性）            |


| クエリ名                              | 説明                                                      |
| --------------------------------- | ------------------------------------------------------- |
| **Outbound Control Rights**       | 他ユーザーに対して制御権を持つユーザー（`GenericAll`, `WriteDACL` など）       |
| **Users with DCSync Rights**      | DCSync 攻撃が可能なユーザー（`Replicating Directory Changes` 権限持ち） |
| **Sessions**                      | 現在ログオンしているセッション（横展開候補）                                  |
| **Group Memberships**             | ユーザーが所属しているグループ一覧                                       |
| **Computers with RDP Enabled**    | RDP接続が可能なホスト一覧                                          |
| **Users with Local Admin Access** | ローカル管理者権限を持っているユーザー                                     |


|アイコン|種類|説明|
|---|---|---|
|👤 緑色の人物アイコン|ユーザー (User)|AD内の個人ユーザーアカウント|
|👥 黄色の集団アイコン|グループ (Group)|ユーザーの所属グループ。強力なグループなら権限昇格の鍵になる|
|🖥️ 赤色のPCアイコン|コンピュータ (Computer)|ADドメインに属するPCやサーバー。DCも含む|
|🌐 水色の地球アイコン|ドメイン (Domain)|ADドメインそのもの|
|📄 紫のポリシーアイコン|GPO (Group Policy Object)|グループポリシー。リンクされることで影響を及ぼす|