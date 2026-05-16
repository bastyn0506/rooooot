## 🧾 Hashcat とは？

- **目的**：パスワードハッシュの高速クラッキング（辞書・ブルートフォース・ルール・マスク攻撃）
    
- **対応形式**：NTLM / bcrypt / md5crypt / sha512crypt / Kerberos / Office / ZIP / WPA2 など超多数
    
- **特徴**：GPUを使って爆速解析、カスタム辞書やマスクにも対応

hashcat -m <ハッシュモード番号> <ハッシュファイル> <攻撃モード> [辞書ファイル or マスク]
hashcat -m 1000 hashes.txt rockyou.txt

| モード番号      | ハッシュ形式           | 説明                 |
| ---------- | ---------------- | ------------------ |
| 0          | MD5              |                    |
| 100        | SHA1             |                    |
| 1400       | SHA256           |                    |
| 1000       | NTLM             | Windows NTハッシュ     |
| 500        | md5crypt         | Unix `$1$`         |
| 1800       | SHA512crypt      | Unix `$6$`         |
| 13100      | Kerberos 5 TGS   | CTFでよく出る           |
| 2500/22000 | WPA/WPA2 (Wi-Fi) | HCCAPX形式 / PMKID対応 |
| 17220      | MS Office 2013+  | Office文書クラッキング     |
| 攻撃法        | 種類               | 内容                 |
| 0          | ストレート            | 辞書攻撃（wordlist）     |
| 1          | 結合               | 辞書 + 辞書            |
| 3          | マスク              | 文字列パターン攻撃          |
| 6          | 辞書 + マスク         | 辞書＋パターン            |
