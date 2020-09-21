<!-- Table of Contents -->
- [はじめに](#はじめに)
- [Node-REDパレット管理](#Node-REDパレット管理)
    - [ノード追加](#ノード追加)
- [Node-REDフロー作成](#Node-REDフロー作成)
    - [injectノード](#injectノード)
    - [text to speechノード](#text-to-speechノード)
    - [play audioノード](#play-audioノード)
    - [functionノード](#functionノード)
    - [ノード接続](#ノード接続)
    - [少しコーディング](#少しコーディング)
    - [音声設定](#音声設定)
- [Text-To-Speech-API準備](#Text-To-Speech-API準備)
    - [リソースの作成](#リソースの作成)    
    - [API情報表示](#API情報表示)
- [Node-REDへText to Speech組み込み](#Node-REDへText-to-Speech組み込み)
    - [API鍵の組み込み](#API鍵の組み込み)
    - [EndPoint-URLの組み込み](#EndPoint-URLの組み込み)
- [Node-REDでWatson-Text to Speech体験](#Node-REDでWatson-Text-to-Speech体験)
    - [流し込むテキストと音声の言語設定](#流し込むテキストと音声の言語設定)
    - [デプロイ](#デプロイ)
- [終わりに](#終わりに)

<!-- Table of Contents -->

## はじめに

このドキュメントは Node-RED を利用して、IBM Watson Text to Speech を体験するための手順を示しています。
ここで利用する Node-RED は、IBM Cloud にデプロイされたものを想定しています。

## Node-REDパレット管理

音声を再生するためのノードを、Node-RED に組み込みます。

### ノード追加
右上の「ハンバーガーメニュー」をクリックし、`「パレット管理」` をクリックします。
![](./img/010.png)

パレット設定画面で `「ノードを追加」` タブをクリックします。
![](./img/020.png)

`ノードを検索` テキストボックスに **play** と入力すると候補が表示されます。  
**node-red-contrib-play-audio** の `「ノードを追加」` ボタンをクリックします。

![](./img/040.png)

確認ダイアログが表示されるので、`「追加」` ボタンをクリックします。

![](./img/050.png)

しばらくするとノードが追加されたことを表すメッセージが表示されます。

![](./img/060.png)


## Node-REDフロー作成

英語テキストの音声を確認してみます。

### injectノード
パレット上部の「共通 タブ」から `inject` ノードをワークスペースにドラッグします。
![](./img/100.png)

### text to speechノード
パレット下部の「IBM Watson タブ」から `text to speech` ノードをワークスペースにドラッグします。

![](./img/110.png)

### play audioノード
パレット上部の「出力 タブ」から `play audio` ノードをワークスペースにドラッグします。

![](./img/120.png)

### functionノード
パレット上部の「機能 タブ」から `function` ノードをワークスペースにドラッグします。

![](./img/130.png)


### ノード接続
`[inject]`⇒`[function]`⇒`[text to speech]`⇒`[play audio]` の順にノードを接続します。

![](./img/150.png)

### 少しコーディング
Watson に話してもらう英文を記述します。

`function` ノードをダブルクリックし、ソースコード編集画面を開きます。そして下記のコードを記述します。

```
msg.payload = "I have a pen.";
return msg;
```
![](./img/160.png)

### 音声設定
出力する音声を設定します。 `play audio` ノードをダブルクリックし、TTSボイスを **English** に設定します。
![](./img/165.png)


## Text-To-Speech-API準備
いったん Node-RED から離れて、IBM Cloud の GUI から `Watson Text to Speech` API を準備します。

### リソースの作成
IBM Cloud のダッシュボード、もしくはリソース・リストにある `「リソースの作成」ボタン`をクリックします。
![](./img/200.png)

検索テキストボックスに `text` と入力すると、**[Text to Speech]** の候補が表示されるのでクリックします。
![](./img/210.png)

サービスの設定画面で **ライト 無料** となっていることを確認して、`「作成」ボタン`をクリックします。
![](./img/220.png)

### API情報表示
作成された API の詳細画面が表示されるので、左メニュの `「管理」` をクリックします。
![](./img/230.png)



## Node-REDへText to Speech組み込み
Node-RED と Watson API を連携させる方法はいくつかあります。

- API 鍵による連携
- Cloud Foundry アプリケーションの接続

今回は「API 鍵による連携」で Node-RED と Watson API を連携します。

### API鍵の組み込み
Watson API の管理画面から `資格情報` の `「API 鍵」` をコピーします。
![](./img/240.png)

Node-RED フローエディタを表示し、ワークスペースに配置してある`[text to speech]`ノードをダブルクリックします。   
**API key** を入力する欄があるので、貼り付けます。
![](./img/250.png)

### EndPoint-URLの組み込み
Watson API の管理画面から `資格情報` の `「URL」` をコピーします。
![](./img/260.png)

Node-RED フローエディタを表示し、ワークスペースに配置してある`[text to speech]`ノードをダブルクリックします。  
**Service Endpoint** を入力する欄があるので、貼り付けます。
![](./img/270.png)

## Node-REDでWatson-Text to Speech体験
Node-RED と Watson Text to Speech の連携は完了したので、Watson の機能を試してみます。

### 流し込むテキストと音声の言語設定
引き続き `[text to speech]`ノードの設定画面で下記設定をします。

- **Language** を `US English`
- **Voice** を `EmilyV3` 


![](./img/280.png)

### デプロイ
いままで設置してきた フローやコーディングを確定して、実際に動作するようにします。画面上部の `「デプロイ」`ボタンをクリックします。

![](./img/290.png)

## 実行
ワークスペースに配置してある `inject` ノードの左側のボタンをクリックして、フローを実行します。すると `function`ノードに記述した英文が `Text to Speech` に送信され、音声データがパソコンから聞こえてきます。
![](./img/300.png)

## 終わりに
文字列を設定するだけで簡単にテキスト音声を得られることが確認できたかと思います。  
「日本語」「英語」以外にも翻訳可能な言語があります。  
いろいろなアプリやサービスの中に気軽に音声読み上げ機能を追加することが可能になります。試してみてはいかがでしょうかっ！