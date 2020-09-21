<!-- Table of Contents -->
- [はじめに](#はじめに)
- [Node-REDフロー作成](#Node-REDフロー作成)
    - [injectノード](#injectノード)
    - [language translatorノード](#language-translatorノード)
    - [functionノード](#functionノード)
    - [debugノード](#debugノード)
    - [ノード接続](#ノード接続)
    - [少しコーディング](#少しコーディング)
- [LanguageTranslator-API準備](#LanguageTranslator-API準備)
    - [リソースの作成](#リソースの作成)    
    - [API情報表示](#API情報表示)
- [Node-REDへLanguageTranslator組み込み](#Node-REDへLanguageTranslator組み込み)
    - [API鍵の組み込み](#API鍵の組み込み)
    - [EndPoint-URLの組み込み](#EndPoint-URLの組み込み)
- [Node-REDでWatson-LanguageTranslator体験](#Node-REDでWatson-LanguageTranslator体験)
    - [翻訳元と翻訳語の言語設定](#翻訳元と翻訳語の言語設定)
    - [デプロイ](#デプロイ)
- [実行](#実行)
    - [英文を変えてみる](#英文を変えてみる)
    - [翻訳の方向を変えてみる](#翻訳の方向を変えてみる)
- [終わりに](#終わりに)

<!-- Table of Contents -->


## はじめに

このドキュメントは Node-RED を利用して、IBM Watson Language Translator の翻訳機能を体験するための手順を示しています。
ここで利用する Node-RED は、IBM Cloud にデプロイされたものを想定しています。


## Node-REDフロー作成

まずは、英語から日本語に翻訳する機能を体験します。

### injectノード
パレット上部の「共通 タブ」から `inject` ノードをワークスペースにドラッグします。
![](./img/010.png)

### language translatorノード
パレット下部の「IBM Watson タブ」から `language translator` ノードをワークスペースにドラッグします。

![](./img/020.png)

### functionノード
パレット上部の「機能 タブ」から `function` ノードをワークスペースにドラッグします。

![](./img/030.png)

### debugノード
パレット上部の「共通 タブ」から `debug` ノードをワークスペースにドラッグします。

![](./img/040.png)

### ノード接続
`[inject]`⇒`[function]`⇒`[language translator]`⇒`[debug]` の順にノードを接続します。

![](./img/050.png)

### 少しコーディング
Watson に問い合わせる英文を準備しておきます。

`function` ノードをダブルクリックし、ソースコード編集画面を開きます。そして下記のコードを記述します。

```
msg.payload = "this is a pen.";
return msg;
```
![](./img/060.png)


## LanguageTranslator-API準備
いったん Node-RED から離れて、IBM Cloud の GUI から `Watson Language Translator` API を準備します。

### リソースの作成
IBM Cloud のダッシュボード、もしくはリソース・リストにある `「リソースの作成」ボタン`をクリックします。
![](./img/100.png)

検索テキストボックスに `language` と入力すると、**[LanguageTranslator]** の候補が表示されるのでクリックします。
![](./img/110.png)

サービスの設定画面で **ライト 無料** となっていることを確認して、`「作成」ボタン`をクリックします。
![](./img/120.png)

### API情報表示
作成された API の詳細画面が表示されるので、左メニュの `「管理」` をクリックします。
![](./img/130.png)



## Node-REDへLanguageTranslator組み込み
Node-RED と Watson API を連携させる方法はいくつかあります。

- API 鍵による連携
- Cloud Foundry アプリケーションの接続

今回は「API 鍵による連携」で Node-RED と Watson API を連携します。

### API鍵の組み込み
Watson API の管理画面から `資格情報` の `「API 鍵」` をコピーします。
![](./img/140.png)

Node-RED フローエディタを表示し、ワークスペースに配置してある`[language translator]`ノードをダブルクリックします。 **API key** を入力する欄があるので、貼り付けます。
![](./img/160.png)

### EndPoint-URLの組み込み
Watson API の管理画面から `資格情報` の `「URL」` をコピーします。
![](./img/170.png)

Node-RED フローエディタを表示し、ワークスペースに配置してある`[language translator]`ノードをダブルクリックします。 **Service Endpoint** を入力する欄があるので、貼り付けます。
![](./img/180.png)

## Node-REDでWatson-LanguageTranslator体験
Node-RED と Watson LanguageTranslator の連携は完了したので、Watson の機能を試してみます。

### 翻訳元と翻訳語の言語設定
引き続き `[language translator]`ノードの設定画面で下記設定をします。

- **Source** を `English`
- **Target** を `Japanese` 


![](./img/190.png)

### デプロイ
いままで設置してきた フローやコーディングを確定して、実際に動作するようにします。画面上部の `「デプロイ」`ボタンをクリックします。

![](./img/200.png)

Node-RED のサイドバー(右側エリア)上部のデバッグアイコンをクリックし、デバッグサイドバーを表示します。
![](./img/210.png)

## 実行
ワークスペースに配置してある `inject` ノードの左側のボタンをクリックして、フローを実行します。すると `function`ノードに記述した英文が `language translator` に送信され、翻訳された結果が `debug` ノードに流れてデバッグサイドバーに表示されます。
![](./img/220.png)

### 英文を変えてみる
ほかの英文も翻訳できることを確認するために、`function` ノードの記述した英文を変更してみます。変更したら、画面右上の **デプロイ**ボタンをクリックしてフローの変更を確定します。
![](./img/230.png)

デプロイが完了したら、`inject` ノードの左側ボタンをクリックして、フローを実行します。翻訳結果が デバッグサイドバーに表示されます。
![](./img/240.png)

### 翻訳の方向を変えてみる
これまで英文を日本語に翻訳していましたが、日本語を英語に翻訳してみます。 `language translator` ノードの設定を下記に変更します。

- **Source** を `Japanese`
- **Target** を `English` 

![](./img/250.png)

翻訳する文字列も日本語にします。`function` ノードを編集して日本語に変更します。変更したら、画面右上の **デプロイ**ボタンをクリックしてフローの変更を確定します。
![](./img/260.png)

デプロイが完了したら、`inject` ノードの左側ボタンをクリックして、フローを実行します。翻訳結果が デバッグサイドバーに表示されます。
![](./img/270.png)

## 終わりに
文字列を設定するだけで簡単に翻訳機能を得られることが確認できたかと思います。  
「日本語」「英語」以外にも翻訳可能な言語があります。  
いろいろなアプリやサービスの中に気軽に翻訳機能を追加することが可能になります。試してみてはいかがでしょうかっ！