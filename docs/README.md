# アプリケーションの設計

扱うツールなどのバージョンは以下の通り。

| Node | npm  | React.js | React Router | Redux | Java   | SpringBoot |
| ---- | ---- | -------- | ------------ | ----- | ------ | ---------- |
|      |      | 16.x.x   |              |       | 11.x.x |            |



## 機能一覧

- 写真の一覧を表示する
- 写真を投稿する（会員のみ）
- 写真にいいねを付ける（会員のみ）
- 写真からいいねを外す（会員のみ）
- 写真にコメントを追加する（会員のみ）
- 写真に付けられたいいねの数を表示する
- 写真に付けられたコメントを表示する
- 会員登録する
- ログインする
- ログアウトする（会員のみ）



### テーブル定義

※ データベースに PostgreSQL を使用するので、自動採番列は SERIAL 型となる。



#### photos テーブル

写真テーブルはユーザーテーブルへの外部キーのほか、ファイル名を表す `filename` カラムを持つ。

ファイル自体は AWS S3 に格納する。

`id` カラムはランダムな文字列とする。

| 列名       | 型           | PRIMARY                                                      | UNIQUE                                                       | NOT NULL                                                     | FOREIGN                                                      |
| ---------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| id         | VARCHAR(255) | ![🔑](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f511.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |                                                              |
| user_id    | INTEGER      |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![➡](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/27a1.png) users(id) |
| filename   | VARCHAR(255) |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |                                                              |
| created_at | TIMESTAMP    |                                                              |                                                              |                                                              |                                                              |
| updated_at | TIMESTAMP    |                                                              |                                                              |                                                              |                                                              |



#### comments テーブル

コメントテーブルはユーザーと写真テーブルへの外部キーに加えてコメント内容を表す `content` カラムを持つ。

| 列名       | 型           | PRIMARY                                                      | UNIQUE                                                       | NOT NULL                                                     | FOREIGN                                                      |
| ---------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| id         | SERIAL       | ![🔑](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f511.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |                                                              |
| photo_id   | VARCHAR(255) |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![➡](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/27a1.png) photos(id) |
| user_id    | INTEGER      |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![➡](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/27a1.png) users(id) |
| content    | TEXT         |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |                                                              |
| created_at | TIMESTAMP    |                                                              |                                                              |                                                              |                                                              |
| updated_at | TIMESTAMP    |                                                              |                                                              |                                                              |                                                              |



#### likes テーブル

いいねテーブルは実質ユーザーと写真テーブルへの外部キーのみを持つ。

| 列名       | 型           | PRIMARY                                                      | UNIQUE                                                       | NOT NULL                                                     | FOREIGN                                                      |
| ---------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| id         | SERIAL       | ![🔑](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f511.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |                                                              |
| photo_id   | VARCHAR(255) |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![➡](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/27a1.png) photos(id) |
| user_id    | INTEGER      |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![➡](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/27a1.png) users(id) |
| created_at | TIMESTAMP    |                                                              |                                                              |                                                              |                                                              |
| updated_at | TIMESTAMP    |                                                              |                                                              |                                                              |                                                              |



#### users テーブル

| 列名       | 型           | PRIMARY                                                      | UNIQUE                                                       | NOT NULL                                                     | FOREIGN |
| ---------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------- |
| id         | SERIAL       | ![🔑](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f511.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |         |
| name       | VARCHAR(255) |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |         |
| email      | VARCHAR(255) |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |         |
| password   | VARCHAR(255) |                                                              |                                                              | ![✅](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/2705.png) |         |
| created_at | TIMESTAMP    |                                                              |                                                              |                                                              |         |
| updated_at | TIMESTAMP    |                                                              |                                                              |                                                              |         |



### ER図

以下がここまでの内容をまとめた ER 図

![ER](https://github.com/Kento75/ReactSpringBootApp/blob/master/docs/assets/ER.png)



## HTTPメソッドの種別・用途

| メソッド | 意味               |
| -------- | ------------------ |
| GET      | リソースの取得     |
| POST     | リソースの新規作成 |
| PATCH    | リソースの一部変更 |
| PUT      | リソースの置き換え |
| DELETE   | リソースの削除     |



### URL 一覧

#### API

![🔒](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f512.png) マークがついている URL は認証済みでないとアクセス不可。

| URL                           | メソッド | 認証                                                         | 内容              |
| ----------------------------- | -------- | ------------------------------------------------------------ | ----------------- |
| /api/photos                   | GET      |                                                              | 写真 一覧取得     |
| /api/photos                   | POST     | ![🔒](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f512.png) | 写真 投稿         |
| /api/photos/{写真ID}          | GET      |                                                              | 写真 詳細取得     |
| /api/photos/{写真ID}/like     | PUT      | ![🔒](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f512.png) | 写真 いいね追加   |
| /api/photos/{写真ID}/like     | DELETE   | ![🔒](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f512.png) | 写真 いいね解除   |
| /api/photos/{写真ID}/comments | POST     | ![🔒](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f512.png) | 写真 コメント追加 |
| /api/register                 | POST     |                                                              | 会員登録          |
| /api/login                    | POST     |                                                              | ログイン          |
| /api/logout                   | POST     | ![🔒](https://cdn.jsdelivr.net/emojione/assets/4.0/png/32/1f512.png) | ログアウト        |
| /api/user                     | GET      |                                                              | 認証ユーザー取得  |

 

#### API 以外

API 以外にサーバサイドで用意する必要がある URL は以下の通り。

| URL                       | メソッド | 認証 | 内容                   |
| ------------------------- | -------- | ---- | ---------------------- |
| /                         | GET      |      | 最初に HTML を返却する |
| /photos/{写真ID}/download | GET      |      | 写真ダウンロード       |



#### フロントエンド

画面側の URLは以下の通り。
フロントエンド（React Router）で実現する。

| URL              | 内容                     |
| ---------------- | ------------------------ |
| /                | 写真一覧ページ           |
| /photos/{写真ID} | 写真詳細ページ           |
| /login           | ログイン・会員登録ページ |

