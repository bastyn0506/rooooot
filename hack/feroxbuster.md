feroxbuster -u https://target.com

feroxbuster -u http://10.10.10.1/ -f -n -x php,html,txt -o feroxbuster/80 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

- `-u, --url <URL>` — ターゲットURL。必須。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/configuration/command-line/?utm_source=chatgpt.com)
    
- `-w, --wordlist <FILE>` — 使用するワードリストを指定（例：`/usr/share/seclists/...`）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/configuration/default-values/?utm_source=chatgpt.com)
    
- `-t, --threads <N>` — 同時スレッド数（デフォルト50）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/configuration/default-values/?utm_source=chatgpt.com)
    
- `-x, --extensions <ext,ext>` — 拡張子を追加して探索（例 `-x php,html,js`）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `-r, --redirects` — リダイレクト先も追跡してスキャンを続ける。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `--no-recursion` / `--depth <N>` — 再帰探索をOFFにするか深さを指定（デフォルト深さは docs 参照）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/configuration/default-values/?utm_source=chatgpt.com)
    
- `-s, --status <codes>` — 残したいステータスコードのみ（例 `-s 200,301,302`）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `--proxy <url>` — プロキシ経由（BurpやSOCKSなど）例：`--proxy http://127.0.0.1:8080` / `--proxy socks5h://127.0.0.1:9050`。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `-H, --header <Header:Value>` — カスタムHTTPヘッダを追加。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `--insecure` — SSL 証明書検証を無視（Burp連携で便利）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `--stdin` — URLリストを標準入力から読む（パイプ処理に便利）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `-o, --output <FILE>` / `--json` — 結果保存（JSON出力も可）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/core-features/?utm_source=chatgpt.com)
    
- `--resume-from <FILE>` — 中断再開。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/configuration/command-line/?utm_source=chatgpt.com)
    
- `--dont-scan <URL|path>` — スキャン除外（deny-list、v2.3.0以降で便利）。[epi052.github.io](https://epi052.github.io/feroxbuster-docs/docs/examples/deny-list/?utm_source=chatgpt.com)