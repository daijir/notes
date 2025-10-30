# Parrot OS ネットワーク検証メモ

```bash
# parrot-test: HTTPサーバー起動
sudo python3 -m http.server 80

# parrot: 接続確認
curl http://192.168.56.103:80/

# parrot-test: 別ターミナルでポート監視
sudo lsof -iTCP:80

# parrot: パケットキャプチャ開始
sudo wireshark &

# parrot: TCP SYNスキャン (ポート接続確認)
sudo nmap -sS -p 80 192.168.56.103
sudo nmap -sS -p 21 192.168.56.103
sudo nmap -sS -p 67 192.168.56.100

# parrot-test: INPUTチェーンをDROPに設定（全パケット破棄）
sudo iptables -P INPUT DROP

# parrot: DROP状態でスキャン（応答なしを確認）
sudo nmap -sS -p 80 192.168.56.103

# parrot-test: INPUTチェーンをACCEPTに戻す
sudo iptables -P INPUT ACCEPT

# parrot: TCPフルコネクトスキャン
nmap -sT -p 80 192.168.56.103

# parrot: UDPスキャン
nmap -sU -p 67 192.167.56.100
```

---

## 補足：RSTパケットとは？

RST（Reset）パケットは、TCP通信中に接続を強制終了するために使用される制御パケットです。TCPヘッダー内の「RST」フラグがセットされていることで識別されます。通常、以下のような状況で送信されます：

- 存在しないポートへのアクセス
- 不正な接続要求
- サーバー側が接続を拒否したい場合

RSTパケットを観察することで、ファイアウォールやサービスの応答状況を把握できます。

---
