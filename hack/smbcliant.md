 smbsmb`smbclient` は、**SMB（Server Message Block）共有に接続・操作するためのLinuxコマンドラインツール**です。Windows共有に「コマンドライン版のFTPクライアント」のように接続して、ファイルのダウンロード・アップロード・列挙などができます。

smbclient //10.10.11.10/{共有名}　-u{ユーザー名}　
smbcliant //10.10.11.10/local -N 匿名アクセス
smbclient -L //10.10.11.35 -N

recurse ON
prompt OFF
mget *     カレントディレクトリのファイル全部ダウンロード


|コマンド|説明|
|---|---|
|`ls`|ファイル一覧表示|
|`cd <フォルダ名>`|ディレクトリ移動|
|`get <ファイル名>`|ダウンロード|
|`put <ファイル名>`|アップロード|
|`mget *`|全部ダウンロード|
|`prompt`|mget時の確認をON/OFF|
|`lcd`|ローカルディレクトリ変更|
|`exit`|終了|
get "Notice from HR.txt"

lcd /home/kali/Desktop/loot　getなどのダウンロード先変更

Cicada$M6Corpb*@Lp#nZp!8

