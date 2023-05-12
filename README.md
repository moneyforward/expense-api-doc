# マネーフォワード クラウド経費APIドキュメント

## 概要

本ドキュメントは[マネーフォワード クラウド経費](https://biz.moneyforward.com/expense)のAPI / [マネーフォワード クラウド債務支払](https://biz.moneyforward.com/payable)のAPIについて説明しております。
各APIのリファレンスはこちらをご覧ください。

- [クラウド経費](https://expense.moneyforward.com/api/index.html)
- [クラウド債務支払](https://payable.moneyforward.com/api/index.html)

ご要望や不具合はメールにてご連絡ください。
詳しいお問い合わせ方法についてはサポートサイト([クラウド経費](https://biz.moneyforward.com/support/expense/guide/support/sup01.html) / [クラウド債務支払](https://biz.moneyforward.com/support/payable/guide/support/sup01.html))をご覧ください。

※ チャットサポートでは対応できかねますので、メールでお問い合わせいただくようお願いいたします。

※ 回答にはお時間いただきますので、あらかじめご了承ください。

## お知らせ

- [API 変更に関するお知らせ](/news/index.md)

## 認証について

アプリケーションの認証はOAuth2.0のAuthorization Code Grantにもとづいて行います。

### アプリケーションの登録

* [マネーフォワード クラウド経費](https://expense.moneyforward.com/session/new) または [マネーフォワード クラウド債務支払](https://payable.moneyforward.com/session/new) にログイン
* 個人設定＞基本設定画面の`API連携（開発者向け）`にある`API連携はこちら`をクリック
* アプリケーションの作成ボタンをクリックし、フォームに必要な情報を入力し、利用規約に同意する、にチェックを入れて作成ボタンをクリックします
* Client IDとClient Secretが発行されます。redirect_uri は https のみ許可しています。

### アクセストークンの発行

* 前項で発行した Client IDとアプリケーション作成時に入力した値を使い、下記のようなURLにアクセスします。
  * ※ 債務支払の場合はドメインを payable.moneyforward.com に読み替えてください

```
https://expense.moneyforward.com/oauth/authorize?client_id=[CLIENT_ID]&redirect_uri=[REDIRECT_URL]&response_type=code&scope=[SCOPE]
```

* アプリケーションを承認すると認証コードつきURLが発行されるので、そのコードを使って以下のようなリクエストをサーバーに発行し、アクセストークン、リフレッシュトークンを取得します。入力に使う[REDIRECT_URL]はアプリケーションの作成時に入力した値を入れてください

```
$ curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d redirect_uri=[REDIRECT_URL] -d grant_type=authorization_code -d code=[認証コード] -X POST https://expense.moneyforward.com/oauth/token
```

※ リクエストを送る際にパラメータは'Content-Type: application/x-www-form-urlencoded'である必要がある点に注意してください。

### APIリクエスト

* 前項で発行したアクセストークンを`Authorization`ヘッダにセットして以下のようなリクエストをサーバーに発行することでAPIを利用できます。

```
$ curl https://expense.moneyforward.com/api/external/v1/offices -H "Authorization: Bearer [ACCESS_TOKEN]"
```
上の例は[事業所一覧を取得するAPI](https://expense.moneyforward.com/api/index.html#!/office/find_offices)をリクエストしています。

### リフレッシュトークンを用いてアクセストークンの再発行

* 前項で取得したリフレッシュトークンを利用して、以下のようなリクエストをサーバーに発行します。

```
$ curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d grant_type=refresh_token -d refresh_token=[REFRESH_TOKEN] -X POST https://expense.moneyforward.com/oauth/token
```

* 上記により、新しいアクセストークンとリフレッシュトークンを取得できます。

### アクセストークンの無効化

* 前項で取得したアクセストークンには有効期限が付いています。即時に無効化したい場合は以下のようなリクエストをサーバーに発行して、アクセストークンを無効化します。

```
$ curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d token=[アクセストークン] -X POST https://expense.moneyforward.com/oauth/revoke
```

### アクセストークンの有効期限の確認

* 前項で取得したアクセストークンが有効かどうか、あるいは有効な場合の有効期限を確認するには、以下のようなリクエストをサーバーに発行します。

```
$ curl https://expense.moneyforward.com/oauth/token/info -H "Authorization: Bearer [アクセストークン]"
```

アクセストークンが有効な場合、サーバーは以下のような JSON レスポンスを返します。

```json
{"resource_owner_id":"12345","scope":["office_setting:write","user_setting:write","transaction:write","report:write","account:write","public_resource:read"],"expires_in":2775961,"application":{"uid":"[CLIENT_ID]"},"created_at":1648021265}
```

ここで `"expires_in": 2775961` が有効期限 (単位は秒) を表し、トークンが無効になるまで残り 2,775,961 秒 (およそ 32 日) であることを表しています。

アクセストークンが無効な場合、サーバーは以下のような JSON レスポンスを返します。

```json
{"error":"invalid_token","error_description":"アクセストークンが無効です","state":"unauthorized"}
```

## APIリファレンス

マネーフォワードクラウド経費のAPIについては[こちら](https://expense.moneyforward.com/api/index.html)

マネーフォワードクラウド債務支払のAPIについては[こちら](https://payable.moneyforward.com/api/index.html)
