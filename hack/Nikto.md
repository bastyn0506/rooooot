- Webサーバーの既知の脆弱性や設定ミスをスキャン
    
- HTTPサーバーのバナー、古いソフト、危険なファイル、CGI などを検出
nikto -h http://TARGET

| オプション             | 説明                 |
| ----------------- | ------------------ |
| `-h`              | ターゲットのURLまたはIP     |
| `-p`              | ポート指定（例：`-p 8080`） |
| `-ssl`            | HTTPSを強制的に使う       |
| `-Tuning`         | スキャンタイプの調整（後述）     |
| `-o result.txt`   | 結果をファイルに保存         |
| `-Format txt      | csv                |
| `-Display V`      | Verbose出力（詳細に）     |
| `-useragent "UA"` | User-Agentの指定      |
| `-id user:pass`   | Basic認証が必要な場合の認証情報 |
