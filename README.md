# マネーフォワード クラウド経費APIドキュメント
## 概要
本ドキュメントは[マネーフォワード クラウド経費](https://biz.moneyforward.com/expense)のAPIについて説明しております。
各APIのリファレンスは[こちら](https://expense.moneyforward.com/api/index.html)をご覧ください。
ご要望や不具合はissue登録にてご連絡ください。

## 認証について
アプリケーションの認証はOAuth2.0のAuthorization Code Grantにもとづいて行います。

### アプリケーションの登録
* [マネーフォワード クラウド経費](https://expense.moneyforward.com/session/new)にログイン
* ホーム下部の`For Developer`の`API連携β（開発者向け）`に進む
* アプリケーションの作成ボタンをクリックし、フォームに必要な情報を入力し、利用規約に同意する、にチェックを入れて作成ボタンをクリックします
* Client IDとClient Secretが発行されます。redirect_uri は https のみ許可しています。

### アクセストークンの発行
* 前項で発行した Client IDとアプリケーション作成時に入力した値を使い、下記のようなURLにアクセスします。
```
https://expense.moneyforward.com/oauth/authorize?client_id=[CLIENT_ID]&redirect_uri=[REDIRECT_URL]&response_type=code&scope=[SCOPE]
```
* アプリケーションを承認すると認証コードつきURLが発行されるので、そのコードを使って以下のようなリクエストをサーバーに発行し、アクセストークン、リフレッシュトークンを取得します。入力に使う[REDIRECT_URL]はアプリケーションの作成時に入力した値を入れてください

```
curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d redirect_uri=[REDIRECT_URL] -d grant_type=authorization_code -d code=[認証コード] -X POST https://expense.moneyforward.com/oauth/token
```

※リクエストを送る際にパラメータは'Content-Type: application/x-www-form-urlencoded'である必要がある点に注意してください。

### APIリクエスト
* 前項で発行したアクセストークンを`Authorization`ヘッダにセットして以下のようなリクエストをサーバーに発行することでAPIを利用できます。

```
curl https://expense.moneyforward.com/api/external/v1/offices -H "Authorization: Bearer [ACCESS_TOKEN]"
```
上の例は[事業所一覧を取得するAPI](https://expense.moneyforward.com/api/index.html#!/office/find_offices)をリクエストしています。

### リフレッシュトークンを用いてアクセストークンの再発行
* 前項で取得したリフレッシュトークンを利用して、以下のようなリクエストをサーバーに発行します。
```
curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d grant_type=refresh_token -d refresh_token=[REFRESH_TOKEN] -X POST https://expense.moneyforward.com/oauth/token
```
* 上記により、新しいアクセストークンとリフレッシュトークンを取得できます。

### アクセストークンの無効化
* 前項で取得したアクセストークンには有効期限が付いていますが、即時に無効化したい場合は以下のようなリクエストをサーバーに発行して、アクセストークンを無効化してください。
```
curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d token=[アクセストークン] -X POST https://expense.moneyforward.com/oauth/revoke
```

## APIリファレンス
https://expense.moneyforward.com/api/index.html
