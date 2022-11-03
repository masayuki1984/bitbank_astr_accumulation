# bitbank_astr_accumulation

## 概要
Bitbankで毎日決められた時間に決められた金額(日本円)分のAstar(ASTR)を購入するBot。
注文時にはPost Onlyを利用することで手数料が掛からないように考慮している。
同時に日本円の残高が一定以下になったらGMOあおぞらネット銀行から設定された金額の振込を行う。

## ユーザーが指定するパラメータ

| パラメータ名 | 設定値 | 説明 |
| ---- | ---- | ---- |
| BUY_HOUR | 0-23 | 購入する時間 |
| BUY_MINUTE | 0-59 | 購入する分 |
| BUY_YEN | 整数値 | 購入する金額(日本円) |

## コア部分の処理プロセスと利用API

#### 1. 取引所ステータスの確認
https://github.com/bitbankinc/bitbank-api-docs/blob/master/rest-api_JP.md#%E5%8F%96%E5%BC%95%E6%89%80%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B

#### 2. 発注済の注文があったらキャンセルを入れる
###### アクティブな注文を取得する
https://github.com/bitbankinc/bitbank-api-docs/blob/master/rest-api_JP.md#%E3%82%A2%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96%E3%81%AA%E6%B3%A8%E6%96%87%E3%82%92%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B

###### 対象通貨ペアのアクティブな注文をキャンセルする
https://github.com/bitbankinc/bitbank-api-docs/blob/master/rest-api_JP.md#%E6%B3%A8%E6%96%87%E3%82%92%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%BB%E3%83%AB%E3%81%99%E3%82%8B%E8%A4%87%E6%95%B0


#### 3. 購入単位の決定
https://github.com/bitbankinc/bitbank-api-docs/blob/master/public-api_JP.md#ticker
ティッカー情報から「現在の買い注文の最高値」を取得する。

#### 4. 指値注文（Post Only）
https://github.com/bitbankinc/bitbank-api-docs/blob/master/rest-api_JP.md#%E6%96%B0%E8%A6%8F%E6%B3%A8%E6%96%87%E3%82%92%E8%A1%8C%E3%81%86
「現在の買い注文の最高値」の価格で指値注文を行う。