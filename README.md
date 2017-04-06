# MFクラウド経費APIドキュメント
## 概要
本ドキュメントは[MFクラウド経費](https://biz.moneyforward.com/expense)のAPIについて説明しております。
各APIのリファレンスは[こちら](https://expense.moneyforward.com/api/index.html)をご覧ください。
ご要望や不具合はissue登録にてご連絡ください。

## 認証について
アプリケーションの認証はOAuth2.0のAuthorization Code Grantにもとづいて行います。

### アプリケーションの登録
* ダッシュボード下部の`For Developer`の`API連携β（開発者向け）`に進む
* アプリケーションの作成ボタンをクリックし、フォームに必要な情報を入力し、利用規約に同意する、にチェックを入れて作成ボタンをクリックします
* Client IDとClient Secretが発行されます


### アクセストークンの発行
* 前項で発行した Client IDとアプリケーション作成時に入力した値を使い、下記のようなURLにアクセスします。
```
https://expense.moneyforward.com/oauth/authorize?client_id=[CLIENT_ID]&redirect_uri=[REDIRECT_URL]&response_type=code&scope=[SCOPE]
```
* アプリケーションを承認すると認証コードつきURLが発行されるので、そのコードを使って以下のようなリクエストをサーバーに発行し、アクセストークンを取得します。入力に使う[REDIRECT_URL]はアプリケーションの作成時に入力した値を入れてください

```
curl -d client_id=[CLIENT_ID] -d client_secret=[CLIENT_SECRET] -d redirect_uri=[REDIRECT_URL] -d grant_type=authorization_code -d code=[認証コード] -X POST https://expense.moneyforward.com/oauth/token
```

※リクエストを送る際にパラメータは'Content-Type: application/x-www-form-urlencoded'である必要がある点に注意してください。

## APIリファレンス
https://expense.moneyforward.com/api/index.html
