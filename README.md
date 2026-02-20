# Capacitor Template

Vue3 + Vuetify3 + Capacitor を使った Android アプリ開発用テンプレート

<img src="./public/icon.png" width="256px" alt="アイコン">

[最新版ダウンロード](https://raw.githubusercontent.com/jikantoki/capacitor-template/refs/heads/main/app-release.apk)

- NOLICENSED ご自由にお使いください

## このプロジェクトについて

Capacitor を使って Vue3 + Vuetify3 アプリを Android アプリとしてパッケージングするためのテンプレートです。  
フロントエンドは Vite でビルドし、バックエンドは PHP で実装しています。  
アカウント管理・プッシュ通知・QR コードなど、アプリに必要な機能をあらかじめ実装済みです。

## 前提

- Node.js（v22 以上推奨）と yarn が入っていること
- PHP と composer が入っていること（バックエンド機能を使う場合）
- Android Studio が入っていること（Android ビルドをする場合）
- MySQL サーバーが用意できること（バックエンド機能を使う場合）

## 使用技術

### フロントエンド

| パッケージ               | 用途                             |
| ------------------------ | -------------------------------- |
| Vue3                     | UIフレームワーク                 |
| Vuetify3                 | UIコンポーネントライブラリ        |
| Vite                     | ビルドツール                     |
| Vue Router               | ルーティング（ファイルベース）    |
| Pinia                    | 状態管理                         |
| TypeScript               | 型安全な開発                     |
| SASS                     | スタイリング                     |
| Capacitor                | Android ネイティブアプリ化        |

### バックエンド

| 技術    | 用途                             |
| ------- | -------------------------------- |
| PHP     | API サーバー                     |
| MySQL   | データベース                     |
| Composer| PHP パッケージ管理               |

## INCLUDED（主な機能）

### Capacitor プラグイン

- カメラ、ファイルシステム、クリップボード、シェア
- ステータスバー制御
- アプリ内ブラウザ
- QR コードリーダー（vue-qrcode-reader）
- QR コード生成（qrcode）
- トースト通知
- デバイス情報取得

### 独自実装

- Ajax API ラッパー（`src/js/ajaxFunctions.ts`）
- 汎用ユーティリティ関数（`src/js/Functions.ts`）
- ダークテーマ切り替え（Vuetify テーマ連携）
- Push 通知 API（`src/js/webpush.ts`）
- アカウント登録・ログイン・パスワードリセット
- メールアドレス認証・アクセストークン発行
- MySQL 用 PHP API 群
- Pinia による状態管理（プロフィール・設定の永続化）

### 設定・開発環境

- VSCode、Git、ESLint、Prettier の設定ファイル
- Vite + unplugin-vue-router によるファイルベースルーティング
- unplugin-vue-components による自動インポート
- Roboto フォント（@fontsource）

## ファイル構成

```text
capacitor-template/
├── android/                  # Capacitor Android プロジェクト
├── php/                      # PHP バックエンド API
│   ├── functions/            # PHP 共通関数
│   ├── createAccount.php     # アカウント作成
│   ├── loginAccount.php      # ログイン
│   ├── resetPassword.php     # パスワードリセット
│   ├── sendPushForAccount.php# プッシュ通知送信
│   ├── updateProfile.php     # プロフィール更新
│   └── ...                   # その他 API
├── public/                   # 静的ファイル
├── runners/                  # Capacitor バックグラウンドランナー
├── src/
│   ├── assets/               # 画像・アイコン等
│   ├── components/           # 共通コンポーネント
│   ├── js/                   # ユーティリティ・API 関数
│   │   ├── Functions.ts      # 汎用関数
│   │   ├── ajaxFunctions.ts  # Ajax API ラッパー
│   │   ├── metaFunctions.ts  # メタ情報関連
│   │   └── webpush.ts        # WebPush 関連
│   ├── mixins/               # Vue ミックスイン
│   ├── pages/                # ページコンポーネント（ファイルベースルーティング）
│   │   ├── index.vue         # ホーム
│   │   ├── login.vue         # ログイン
│   │   ├── registar.vue      # アカウント登録
│   │   ├── settings/         # 設定画面
│   │   ├── user/             # ユーザー関連画面
│   │   └── ...
│   ├── plugins/              # Vue プラグイン設定
│   ├── router/               # ルーター設定
│   ├── stores/               # Pinia ストア
│   │   ├── myProfile.ts      # 自分のプロフィール
│   │   └── settings.ts       # アプリ設定
│   └── styles/               # グローバルスタイル（SCSS）
├── capacitor.config.ts       # Capacitor 設定
├── database.sql              # MySQL スキーマ
├── vite.config.mts           # Vite 設定
└── package.json
```

## 注意

開発サーバーはポート **8989** で動くようにしてあります（`http://localhost:8989`）  
VSCode での利用を推奨

## Setup

このプログラムは、表示用サーバーと処理用サーバーの 2 つが必要です

### 表示用サーバー（フロントエンド）

```shell
git clone git@github.com:jikantoki/capacitor-template.git
cd capacitor-template
yarn install
```

### 環境変数の設定

ルートに `.env` ファイルを作成し、以下のように記述（クォーテーション不要）

```env
VUE_APP_WEBPUSH_PUBLICKEY=パブリックキーをコピー
VUE_APP_WEBPUSH_PRIVATEKEY=プライベートキーをコピー

VUE_APP_API_ID=default
VUE_APP_API_TOKEN=後のPHPで作成するアクセストークン
VUE_APP_API_ACCESSKEY=後のPHPで作成するアクセスキー

VUE_APP_API_HOST=APIサーバーのホスト
```

WebPush 用の鍵はここで作れます: <https://web-push-codelab.glitch.me/>

### PHP サーバー（内部処理用）

サーバーサイドは PHP で開発しているため、一部処理を実行するには PHP サーバーの用意が必要です  
レンタルサーバーでも動作します

1. API 用のドメインをクライアント側とは別で用意する
2. このリポジトリの `php` フォルダをドメインのルートにする（.htaccess 等で）
3. リポジトリルート直下に `/env.php` を用意し、以下の記述をする

```php
<?php
define('DIRECTORY_NAME', '/プロジェクトルートのディレクトリ名');

define('VUE_APP_WebPush_PublicKey', 'パブリックキー');
define('VUE_APP_WebPush_PrivateKey', 'プライベートキー');
define('WebPush_URL', 'プッシュ通知を使うドメイン');
define('WebPush_URL_dev', 'プッシュ通知を使うドメイン（開発用）'); // この行は無くても良い
define('WebPush_icon', 'プッシュ通知がスマホに届いたときに表示するアイコンURL');
define('Default_user_icon', 'アイコン未設定アカウント用の初期アイコンURL');

define('MySQL_Host', 'MySQLサーバー');
define('MySQL_DBName', 'DB名');
define('MySQL_User', 'DB操作ユーザー名');
define('MySQL_Password', 'DBパスワード');

define('SMTP_Name', '自動メール送信時の差出名');
define('SMTP_Username', 'SMTPユーザー名');
define('SMTP_Mailaddress', '送信に使うメールアドレス');
define('SMTP_Password', 'SMTPパスワード');
define('SMTP_Server', 'SMTPサーバー');
define('SMTP_Port', 587); // 基本は587を使えば大丈夫

$mailHeader = "<p>
いつも Capacitor Template をご利用いただきありがとうございます。
<hr>
</p>";
$mailFooter = "<p>
<hr>
このメールに返信することはできません。
<br>
また、このメールに身に覚えのない場合は、エノキ電気までお問い合わせください。
<br>
<a href=\"https://enoki.xyz\">Capacitor Template</a> by <a href=\"https://enoki.xyz\">エノキ電気</a>
</p>";
```

#### PHP サーバー用の .htaccess の用意

```htaccess
# トップページを /capacitor-template/php にする
<IfModule mod_rewrite.c>
RewriteEngine on
RewriteBase /
RewriteRule ^$ capacitor-template/php/ [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.+)$ capacitor-template/php/$1 [L]
</IfModule>
# 外部からのAPIへのアクセスを許可
Header append Access-Control-Allow-Origin: "*"
```

### MySQL の用意

#### `/database.sql` ファイルをインポートする

phpMyAdmin が使える環境なら DB 直下にインポートして終わり

### デフォルト API トークンの発行

このプログラムは独自のアクセストークンを利用して API にアクセスします。  
そのため、初回 API を登録する作業が必要です。

1. セットアップした API 用サーバーの `/makeApiForAdmin.php` にアクセス
2. 初回アクセス時のみ MySQL で登録作業が行われるので、出てきた画面の内容をコピー
3. `.env` に内容を記述（書き方は上述）
4. 以後、その値を使って API を操作できます

> **注意**: 忘れたらリセットするしかありません（一部データは暗号化されており、管理者でも確認できません）

#### デフォルト API トークンのリセット方法

1. MySQL の `api_list` テーブルの `secretId='default'` を削除
2. `api_listForView` の `secretId='default'` も同様に削除
3. 初回登録と同じ手順で再登録

## コマンド

### インストール

```shell
yarn install
composer install # PHP用
```

### 開発サーバー起動

```shell
yarn dev
```

開発サーバーは <http://localhost:8989> で起動します

### ビルド（本番用）

```shell
yarn build
```

### Lint

```shell
yarn lint
```

### Android ビルド

```shell
# Webアセットをビルドしてから Capacitor に同期
yarn build
npx cap sync android
# Android Studio で開く
npx cap open android
```

## 設定方法

| 項目               | 設定箇所                              |
| ------------------ | ------------------------------------- |
| アプリ名           | `/package.json` の `name`             |
| アプリ ID          | `/capacitor.config.ts` の `appId`     |
| ナビゲーション      | `/src/pages/` 以下のページコンポーネント |
| グローバルスタイル  | `/src/styles/`                        |
| Capacitor 設定      | `/capacitor.config.ts`                |

## トラブルシューティング

### PHP がおかしい

composer ちゃんと入れた？

### `yarn dev` でポートが使えない

`vite.config.mts` の `server.port` を変更してください（デフォルト: 8989）

### Android ビルドでエラーが出る

1. `yarn build` が正常に完了しているか確認
2. `npx cap sync android` を再度実行
3. Android Studio の Gradle sync を試す

## 参考資料

- WebPush: <https://tech.excite.co.jp/entry/2021/06/30/104213>
- Capacitor: <https://capacitorjs.com/docs>
- Vuetify: <https://vuetifyjs.com/>
