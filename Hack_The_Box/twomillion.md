# HackTheBox: 2Million Walkthrough

## 1. ポートスキャン

```bash
sudo nmap -p- --min-rate=10000 -oA nmap/twomillion-allports -v 2million.htb
sudo nmap -sC -sV -p22,80 -oA nmap/twomillion 2million.htb
```

## 2. ホスト設定

```bash
sudo vi /etc/hosts
# 追加:
<IP address> 2million.htb
```

## 3. ディレクトリ探索

```bash
feroxbuster -u http://2million.htb/
```

## 4. ログインページの確認

- ブラウザで `http://2million.htb/login?error=<b>Please subscribe</b>` を確認
- 「Join HTB」をクリック
- 開発者ツール → Network → Refresh → Debugger → `inviteapi.min.js` を確認
- JS Beautifier でスクリプトを整形

## 5. Inviteコード生成の流れ

```bash
curl -X POST 2million.htb/api/v1/invite/how/to/generate
curl -X POST 2million.htb/api/v1/invite/how/to/generate | jq .
```

- ブラウザのコンソールで `makeInviteCode()` を実行
- JSONデータをコピー
- CyberChef に貼り付け → `rot13` を適用
- 出力: `In order to generate the invite code, make a POST request to /api/v1/invite/generate`

```bash
curl -X POST 2million.htb/api/v1/invite/generate
curl -X POST 2million.htb/api/v1/invite/generate | jq .
```

- Inviteコードを取得

```bash
echo <invite code> | base64 -d
```

- Inviteコードをデコード

```bash
curl -s -q -X POST 2million.htb/api/v1/invite/generate | jq .data.code -r | base64 -d
```

## 6. サインアップ

- Inviteコードページへ移動
- 情報を入力して登録

## 7. VPNファイルの取得と接続

- 脆弱性を探す → アクセス → VPNファイル取得
- VPNファイル内のドメイン名を確認
- `/etc/hosts` を更新

```bash
sudo openvpn <name>.ovpn
```

- 接続できない場合 → 「Regenerate」クリック → Burpで確認 → Repeaterへ送信

## 8. API探索

```http
GET /api/v1 HTTP/1.1
```

```bash
curl 2million.htb/api/v1 -H 'Cookie: PHPSESSID=....' | jq .
```

## 9. ルート確認

```http
GET api/v1/user/auth
# 結果:
# => login, not admin
# => log out
```

## 10. 管理者権限の取得

- `/register` にアクセス
- メール: `admin@ippsec.rocks`
- パスワード入力
- Burpでリクエストをインターセプト
- パラメータに `&is_admin=1` を追加

---
