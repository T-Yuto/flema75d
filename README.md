# README

## 開発環境

- Git
- Github

- Ruby 2.5.1
- Ruby on Rails 5.2.3
- jQuery

- Database mysql2 0.5.3 開発環境
- S3 本番環境
- AWS EC2

## 開発担当

- DB 設計(メンバー全員で)
- 商品出品機能（サーバーサイド・フロントエンド）
- 商品編集機能（サーバーサイド・フロントエンド）
- お気に入り（いいね）機能（サーバーサイド・フロントエンド）
- ユーザー評価機能（サーバーサイド・フロントエンド）
- カテゴリー一覧ページ（フロントエンド）
- マイページいいね、出品した商品、購入した商品一覧ページ（フロントエンド）

### 主な開発 Branch

- 出品機能
  https://github.com/watcher041/flema75d/pull/16
- 編集機能
  https://github.com/watcher041/flema75d/pull/32
- お気に入り（いいね）機能
  https://github.com/watcher041/flema75d/pull/51
- マイページページネーション非同期
  https://github.com/watcher041/flema75d/pull/54
- ユーザー評価機能
  https://github.com/watcher041/flema75d/pull/64

## ER 図

![ER figure](https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/ER図.png)

## Users テーブル

| Column       | Type   | Options                  |
| ------------ | ------ | ------------------------ |
| nickname     | string | null: false, unique:true |
| email        | string | null: false, unique:true |
| password     | string | null: false, default""   |
| first_name   | string | null: false, default""   |
| last_name    | string | null: false, default""   |
| first_kana   | string | null: false, default""   |
| last_kana    | string | null: false, default""   |
| birthday     | date   | null: false              |
| phone_number | string |                          |
| image        | text   |                          |
| profile      | text   |                          |

### Association

- has_one :address, dependent: :destroy
- has_many :items, dependent: :destroy
- has_many :cards, dependent: :destroy
- has_many :comments ,dependent: :destroy
- has_many :likes, dependent: :destroy
- has_many :addresses, dependent: :destroy
- has_many :cards, dependent: :destroy
- has_many :evaluates, dependent: :destroy
- has_many :sns_credentials, dependent: :destroy

## Address テーブル

| Column        | Type    | Options                |
| ------------- | ------- | ---------------------- |
| zipcode       | string  | null: false, default"" |
| prefecture_id | integer | null: false            |
| city          | string  | null: false, default"" |
| address       | string  | null: false, default"" |
| build_name    | string  |                        |
| user_id       | integer | foreign_key: true      |

### Association

- belongs_to :user, optional: true
- belongs_to_active_hash :prefecture

## Items テーブル

| Column        | Type    | Options                       |
| ------------- | ------- | ----------------------------- |
| name          | string  | null: false                   |
| detail        | text    | null: false                   |
| price         | integer | null: false                   |
| pay_side      | integer | enum, null: false             |
| post_date     | integer | enum, null: false             |
| status        | integer | enum, null: false             |
| situation     | integer | enum, null: false, default: 0 |
| post_way_id   | integer | null: false, default: 0       |
| prefecture_id | integer | null: false                   |
| category_id   | integer | foreign_key: true             |
| brand_id      | integer |                               |
| user_id       | integer | foreign_key: true             |
| buyer_id      | integer |                               |

### Association

- has_many :likes,dependent: :destroy
- has_many :images, dependent: :destroy
- has_many :comments, dependent: :destroy
- has_many :evaluates, dependent: :destroy
- belongs_to :category
- belongs_to :user
- belongs_to :brand, optional: true
- belongs_to_active_hash :prefecture
- belongs_to_active_hash :post_way

## Comments テーブル

| Column  | Type    | Options           |
| ------- | ------- | ----------------- |
| comment | text    | null: false       |
| user_id | integer | foreign_key: true |
| item_id | integer | foreign_key: true |

### Association

- belongs_to :user
- belongs_to :item

## Brands テーブル

| Column | Type   | Options     |
| ------ | ------ | ----------- |
| name   | string | null: false |

### Association

- has_many :items

## Images テーブル

| Column  | Type    | Options           |
| ------- | ------- | ----------------- |
| image   | text    | null: false       |
| item_id | integer | foreign_key: true |

### Association

- belongs_to :item

## Likes テーブル

| Column  | Type    | Options           |
| ------- | ------- | ----------------- |
| user_id | integer | foreign_key: true |
| item_id | integer | foreign_key: true |

### Association

- belongs_to :item
- belongs_to :user

## Categories テーブル

| Column   | Type         | Options     |
| -------- | ------------ | ----------- |
| name     | string       | null: false |
| ancestry | string:index |             |

### Association

- has_many :items

## Cards テーブル

| Column      | Type    | Options                  |
| ----------- | ------- | ------------------------ |
| customer_id | string  | null: false              |
| card_id     | string  | null: false, unique:true |
| user_id     | integer | foreign_key:true         |

### Association

- belongs_to :user

## Evaluates テーブル

| Column      | Type    | Options     |
| ----------- | ------- | ----------- |
| evaluate_id | integer | null: false |
| user_id     | integer | null: false |
| item_id     | integer | null: false |
| rate        | integer | null: false |

### Association

- belongs_to :user
- belongs_to :item

## SnsCredentials テーブル

| Column   | Type       | Options          |
| -------- | ---------- | ---------------- |
| provider | string     | null: false      |
| uid      | string     | null: false      |
| user_id  | references | foreign_key:true |

### Association

- belongs_to :user, optional: true

## 担当詳細

### 商品出品ページ

#### 概要

- 商品の出品（新規作成）をするページ

#### 担当内容（フロントエンド）

- 情報を入力する欄が表示される
- HTML、CSS、JavaScript を使用してのマークアップ作業

#### 担当内容（サーバーサイド）

- 出品時の モデル、コントローラー、ルーティング の作成
- 未ログイン時ユーザー登録画面へ遷移される
- 画像が 1 枚から 10 枚まで登録できる
- 必須項目が空だと登録できないようなvalidationの設定

### 商品編集ページ

#### 概要

- 出品した商品を編集できる機能

#### 担当内容（フロントエンド）

- 出品時と同じビューを使用
- 編集時には”出品する”ボタンを”変更する”と表示
- 全ての項目が入力された状態で表示

#### 担当内容（サーバーサイド）

- 出品したユーザーと一致するユーザーでないと商品詳細ページに遷移される
- 画像が 1 枚から 10 枚以下でないと更新できないようにコントローラーで制御
- カテゴリーの親情報を取得する機能を実装

### お気に入り（いいね）機能

#### 概要

- 出品された商品をお気に入りに登録する機能

#### 担当内容（フロントエンド）

- Ajax 通信を用いて、お気に入り登録時と解除時のビューを制御
- 自身の出品した商品をお気に入り登録できないように制御
- 商品一覧においてお気に入り登録しているかどうかで、♡ の色を制御

#### 担当内容（サーバーサイド）

- お気に入り機能のモデル、コントローラー、ルーティングの作成

#### 概要

- お気に入り登録した商品がわかるように、マイページにいいね一覧ページを作成

#### 担当内容（フロントエンド）

- いいねした商品の画像と名前と値段が確認できる。

#### 担当内容（サーバーサイド）

- gem “kaminari” を使用し、ページネーション機能を実装 8 件ずつ表示させる

### ユーザー評価機能

#### 概要

- 購入した商品のユーザーに対して、３段階での評価とコメントで評価する機能

#### 担当内容（フロントエンド）

- 評価するためのページを HTML、CSS、JavaScript を使用して作成
- マイページの top 画面に評価された数を表示
- マイページの購入した商品一覧に評価とコメントを表示
- 商品詳細ページのユーザーの欄に３段階で評価された数を表示

#### 担当内容（サーバーサイド）

- 商品と評価されたユーザーを紐付けて enum を用いて３段階で保存する機能の実装
- すでに評価した商品に対して評価できないようにコントローラーで制御

### カテゴリー一覧ページ

#### 概要

- カテゴリーの一覧を表示するページ

#### 担当内容（フロントエンド）

- HTML、CSS、JavaScript を使用して作成
- JavaScript を使用して、選択したカテゴリーへスクロールするように作成

## 商品出品・編集画面

<img alt= "item new" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/商品出品ページ1.png" width= "500px">
<img alt= "item new" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/商品出品ページ2.png" width= "500px">
<img alt= "item new" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/商品出品ページ3.png" width= "500px">

## お気に入り（いいね）機能

<img alt= "like click" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/商品詳細ページいいねボタン.png" width= "500px">
<img alt= "like click" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/商品詳細ページいいねボタンクリック後.png" width= "500px">

## マイページいいね・出品・購入一覧

#### 出品一覧

<img alt= "new list" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページ出品した商品一覧出品なし.png" width= "500px">
<img alt= "new list" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページ出品した商品一覧出品あり.png" width= "500px">

#### 購入一覧

<img alt= "buy list" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページ購入した商品一覧ない時.png" width= "500px">
<img alt= "buy list" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページ購入した商品一覧ある時.png" width= "500px">

#### いいね一覧

<img alt= "like list" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページいいね一覧いいねなし.png" width= "500px">
<img alt= "like list" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページいいね一覧いいねあり.png" width= "500px">

## ユーザー評価機能

<img alt= "user rank" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/商品のユーザーへの評価リンク説明.png" width= "500px">
<img alt= "user rank" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/ユーザー評価詳細.png" width= "500px">
<img alt= "user rank" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/ユーザー評価数確認説明.png" width= "500px">
<img alt= "user rank" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/マイページ評価数確認.png" width= "500px">

## カテゴリー一覧表示

#### スクロール

<img alt= "category scroll" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/カテゴリー一覧上部説明.png" width= "500px">
<img alt= "category scroll" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/カテゴリー一覧スクロール説明.png" width= "500px">
<img alt= "category link" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/カテゴリー一覧リンク説明１.png" width= "500px">
<img alt= "user rank" src= "https://raw.githubusercontent.com/T-Yuto/flema75d/Readme/Readme/categoryscroll.gif" width= "500px">
