# この部分は開発環境で必要。開発サーバーの立ち上げでもnodeいる。ビルド時にもnpm buildがいる
FROM node:18 as base

CMD [ "bash" ]

# マルチステージビルド
#build(本番環境では不要な内容を削ったりなどする、html.jsを抽出)
from base as build
copy . /workspace
# ファイル群がある場所に移動
workdir /workspace
# Dockerfileの中でだけ有効　githubアクションの中でパラメーターをつける
# npm run buildで使うだけなのでargでいい
ARG REACT_APP_API_SERVER
# npm installより厳密にバージョン管理ができるやつ。package.jsonをインストールするのは同じ
run npm ci && npm run build

#hosting in nginx(nodeここからもういらん) ベースのやつは使わん。nodeもういらんから。buildした成果物だけでいい
from nginx:latest as production
# ローカルだけではなく他のビルドからも取ってこれる　/var/wwwはnginxの作法　この中のやつがhttpサーバーで通信できるようになる
copy --from=build /workspace/build /var/www
# ディレクトリ指定　この中におくと設定として使ってくれる
copy nginx.conf /etc/nginx/conf.d/default.conf
# コマンドは設定しない　デフォルトコマンドはnginxの起動だがそれでいい
# docker-compose.ymlは今回ではローカル環境でのみ使われる。だからデプロイ時はその中身の設定は別途必要　