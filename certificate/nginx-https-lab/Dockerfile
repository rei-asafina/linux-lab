FROM nginx:alpine

# 証明書置き場を作る
RUN mkdir -p /etc/nginx/certs

# 設定と証明書を配置
COPY nginx.conf /etc/nginx/nginx.conf
COPY server.crt server.key /etc/nginx/certs/

# コンテナがリッスンするポート
# 80: HTTP 通信のためのポート
# 443: HTTPS 通信のためのポート

EXPOSE 80 443
