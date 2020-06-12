# README

## 開発環境
Ruby  2.5.1
Ruby on Rails  5.2.3
MySQL  mysql2 0.5.3
Github
AWS
VS Code

## 開発担当
・商品出品機能
・商品編集機能
・お気に入り（いいね）機能
・ユーザー評価機能
・カテゴリー一覧表示機能
・マイページ 出品、購入、いいね一覧表示（非同期通信、ページネーション）

### 主な開発Branch
出品機能
https://github.com/watcher041/flema75d/pull/16
編集機能
https://github.com/watcher041/flema75d/pull/32
お気に入り（いいね）機能
https://github.com/watcher041/flema75d/pull/51
マイページページネーション非同期
https://github.com/watcher041/flema75d/pull/54
ユーザー評価機能
https://github.com/watcher041/flema75d/pull/64


## ER図
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

- has_one  :address, dependent: :destroy
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

### 商品出品ページ
・概要
　・商品の出品（新規作成）をするページ
・担当内容（フロントエンド）
　・情報を入力する欄が表示される
　・HTML、CSS、JavaScriptを使用してのマークアップ作業
・担当内容（サーバーサイド）
　・出品時の モデル、コントローラー、ルーティング の作成
　・未ログイン時ユーザー登録画面へ遷移される
　・画像が1枚から10枚まで登録できる

### 商品編集ページ　リンク
・概要
　・出品した商品を編集できる機能
・担当内容（フロントエンド）
　・出品時と同じビューを使用
　・編集時には”出品する”ボタンを”変更する”と表示
　・全ての項目が入力された状態で表示
・担当内容（サーバーサイド）
　・出品したユーザーと一致するユーザーでないと商品詳細ページに遷移される
　・画像が1枚から10枚以下でないと更新できないようにコントローラーで制御
　・カテゴリーの親情報を取得する機能を実装

### お気に入り（いいね）機能
・概要
　・出品された商品をお気に入りに登録する機能
・担当内容（フロントエンド）
　・Ajax通信を用いて、お気に入り登録時と解除時のビューを制御
　・自身の出品した商品をお気に入り登録できないように制御
　・商品一覧においてお気に入り登録しているかどうかで、♡の色を制御
・担当内容（サーバーサイド）
　・お気に入りのモデル、コントローラー、ルーティングの作成
・概要
　・お気に入り登録した商品がわかるように、マイページにいいね一覧ページを作成
・担当内容（フロントエンド）
　・いいねした商品の画像と名前と値段が確認できる。
・担当内容（サーバーサイド）
　・gem “kaminari” を使用し、ページネーション機能を実装　8件ずつ表示させる

### ユーザー評価機能
・概要
　・購入した商品のユーザーに対して、３段階での評価とコメントで評価する機能
・担当内容（フロントエンド）
　・評価するためのページをHTML、CSS、JavaScriptを使用して作成
　・マイページのtop画面に評価された数を表示
　・マイページの購入した商品一覧に評価とコメントを表示
　・商品詳細ページのユーザーの欄に３段階で評価された数を表示
・担当内容（サーバーサイド）
　・商品と評価されたユーザーを紐付けてenumを用いて３段階で保存する機能の実装
　・すでに評価した商品に対して評価できないようにコントローラーで制御

### カテゴリー一覧ページ
・概要
　・カテゴリーの一覧を表示するページ
・担当内容（フロントエンド）
　・HTML、CSS、JavaScriptを使用して作成
　・JavaScriptを使用して、選択したカテゴリーへスクロールするように作成

