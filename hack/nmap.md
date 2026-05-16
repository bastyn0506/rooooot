Nmap Scripting Engineのスクリプトを利用して、バナーグラビング実行に利用できます。このスクリプトはホスト上で検出したすべてのサービスのバナーを捕捉しようと試みます。次のコマンドでスクリプトが起動されます：nmap -sV --script=banner <ターゲットアドレス>。このスクリプトの利用で、ペンテスト担当者は大量の情報を入手します。


-p 22  　　　　　　　特定ポート
-p-　　　　　　　　　全ポート
--top-ports 100　　　よく使われる上位100ポート
-p1-10000            1から１００００まで

-sS → SYNスキャン（デフォルト・ステルス）
-sU → UDPスキャン
-sA → ACKスキャン（FW検出）
-sI zombieIP → Idleスキャン

-sV → サービスバージョンを検出
--version-intensity <0-9> → 検出の強度
--version-trace → デバッグ出力付き

-O → OS検出
-sC → デフォルトスクリプト実行
-T4 → Aggressive（速め、よく使う）
--min-rate 1000 → 1秒あたり最低1000パケット

-Pn → Pingせず直接スキャン
--open → 開いているポートのみ表示
-v / -vv → 詳細表示
-A   -O -sV -sC --traceroute のまとめ
-sC → デフォルトスクリプト実行

nmap -p- -Pn  -T4 --min-rate 1000 <IP>  全ポート確認


nmap -A -p 22,80,445 <IP> 絞って詳細
