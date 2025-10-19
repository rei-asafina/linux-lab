### 実験概要

自前の自己署名証明書 (self-signed certificate) を使用して HTTPS を立て、クライアント側で「SSL certificate problem: self signed certificate」というエラーを発生させる。このエラーは、自己署名証明書が信頼された認証局 (CA) によって署名されていないために発生する。

### SAN(Subject Alternative Name) 付き自己署名証明書を作る

SAN を入れておくと 「ホスト名不一致」系のエラーは回避 できる（“発行者が信頼できない”という別のエラーだけが出る状態に持っていける）

#### server.key(秘密鍵) + server.crt(自己署名証明書)

```
openssl req -newkey rsa:2048 -nodes -keyout server.key \
  -x509 -days 30 -out server.crt \
  -subj "/C=JP/ST=Osaka/L=Osaka/O=Lab/OU=Dev/CN=localhost" \
  -addext "subjectAltName=DNS:localhost,IP:127.0.0.1"
```

### ビルド＆起動

```
docker build -t nginx-https-lab .
docker run --rm -p 8080:80 -p 443:443 --name https-lab nginx-https-lab
```

### 動作チェック

```
検証付きでcurl
curl -v https://localhost:8443

* SSL certificate problem: self-signed certificate

検証をスキップしてcurl（-kでSSLのエラーを無視して処理を継続）
curl -vk https://localhost:8443
hello over https
```
