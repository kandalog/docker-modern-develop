# Qiita公開用リポジトリ

## 目的
SPAのPFを作成しようと考えた際、Rails7 Mysql8 React TypeScriptの環境が欲しかった
既存の記事を10サイト以上探し回ったが、惜しい記事はあれど自分が欲しているものは見つからなかった
そのため、自分で空のコンテナを立てて欲しい環境を手探りで構築した
慣れないDocker環境の構築とエラーとの戦いを経て、完成させることができたので、誰かの役に立てればと公開する
Qiitaでも公開しているが、確認用としてGitHubにも掲載しておく

## 使い方
リポジトリをcloneした後docker-compose.ymlがある階層で下記の順で作業してください

## コマンド

### バックエンド
```shell
# コンテナ内で railsアプリを作成
docker compose run --rm backend rails new . --force --no-deps -d mysql --api
```

```yml
# config/database.ymlを編集してコンテナのmysqlに接続する
password: root
host: db
```
```shell
# Gemfileが更新されたので再度buildする
docker compose build

# DBを作成
docker compose run --rm backend rails db:create
```

### フロントエンド

```shell
docker compose run --rm frontend sh -c "mv Dockerfile .. && npx create-react-app . && mv /Dockerfile /frontend/Dockerfile"
```

### 全てのコンテナを立ち上げる
```shell
docker compose up -d
```

*`http://localhost:3000`* → `rails`
*`http://localhost:3001`* → `react`

## 知識
`docker comose run <サービス> <コマンド>`で複数のコマンドを実行したい時
`sh -c "A && B"`とする必要がある
`A && B`とすると2個目以降はホストで実行されてしまう。

`yml`に`command`が設定されている場合
`docker compose run`や`docker compose up`では`Dockerfile`の`CMD`は無視される
`docker container <Image>`名で起動する場合は`CMD`
`docker compose run <コマンド>`とする場合は設定したコマンドを優先する
