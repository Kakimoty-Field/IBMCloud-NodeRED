<!-- Table of Contents -->
- [はじめに](#はじめに)
- [Node-REDパレット管理](#Node-REDパレット管理)
    - [ノード追加](#ノード追加)
      - [node-red-contrib-browser-utils](#node-red-contrib-browser-utils)
      - [node-red-contrib-image-output](#node-red-contrib-image-output)
- [Node-REDフロー作成](#Node-REDフロー作成)
    - [cameraノード](#cameraノード)
    - [imageノード](#imageノード)
    - [visual recognitionノード](#visual-recognitionノード)
    - [debugノード](#debugノード)
    - [デバッグ設定](#デバッグ設定)
    - [ノード接続](#ノード接続)
- [Visual-Recognition-API準備](#Visual-Recognition-API準備)
    - [リソースの作成](#リソースの作成)    
    - [API情報表示](#API情報表示)
- [Node-REDへVisual Recognition組み込み](#Node-REDへVisual-Recognition組み込み)
    - [API鍵の組み込み](#API鍵の組み込み)
    - [EndPoint-URLの組み込み](#EndPoint-URLの組み込み)
- [Node-REDでWatson Visual Recognition体験](#Node-REDでWatson-Visual-Recognition体験)
    - [画像認識結果の言語設定](#画像認識結果の言語設定)
    - [デプロイ](#デプロイ)
- [実行](#実行)
- [終わりに](#終わりに)

<!-- Table of Contents -->

## はじめに

このドキュメントは Node-RED を利用して、IBM Watson Visual Recognition の **画像認識** を体験するための手順を示しています。  
ここで利用する Node-RED は、IBM Cloud にデプロイされたものを想定しています。

## Node-REDパレット管理

下記のノードを追加します。

- **[node-red-contrib-browser-utils]**  
ブラウザでカメラを利用するためのノード

- **[node-red-contrib-image-output]**  
Node-RED のフローを流れるイメージを表示するノード
 　

### ノード追加
右上の「ハンバーガーメニュー」をクリックし、`「パレット管理」` をクリックします。
![](./img/010.png)

パレット設定画面で `「ノードを追加」` タブをクリックします。
![](./img/020.png)

#### node-red-contrib-browser-utils
`ノードを検索` テキストボックスに **browser** と入力すると候補が表示されます。  
**node-red-contrib-browser-utils** の `「ノードを追加」` ボタンをクリックします。

![](./img/030.png)

確認ダイアログが表示されるので、`「追加」` ボタンをクリックします。

![](./img/040.png)

しばらくするとノードが追加されたことを表すメッセージが表示されます。

![](./img/050.png)

#### node-red-contrib-image-output
`ノードを検索` テキストボックスに **image-out** と入力すると候補が表示されます。  
**node-red-contrib-image-output** の `「ノードを追加」` ボタンをクリックします。

![](./img/060.png)

確認ダイアログが表示されるので、`「追加」` ボタンをクリックします。

![](./img/070.png)

しばらくするとノードが追加されたことを表すメッセージが表示されます。

![](./img/080.png)



## Node-REDフロー作成

英語テキストの音声を確認してみます。

### cameraノード
パレット中段の「入力 タブ」から `camera` ノードをワークスペースにドラッグします。
![](./img/100.png)

### imageノード
パレット中段の「出力 タブ」から `image` ノードをワークスペースにドラッグします。
![](./img/110.png)

### visual recognitionノード
パレット下部の「IBM Watson タブ」から `visual recognition` ノードをワークスペースにドラッグします。
![](./img/120.png)

### debugノード
パレット上部の「共通 タブ」から `debug` ノードをワークスペースにドラッグします。
![](./img/130.png)

### デバッグ設定
画像認識結果をデバッグサイドバーに表示するために、`debug` ノードの設定を変更します。  
`debug` ノードをダブルクリックし、デバッグの `対象`を **msg.result** になるように設定します
![](./img/150.png)

### ノード接続
`[camera]`⇒`[image]`  
`[camera]`⇒`[visual recognition]`⇒`[debug]` の順にノードを接続します。
![](./img/160.png)


## Visual-Recognition-API準備
いったん Node-RED から離れて、IBM Cloud の GUI から `Visual Recognition` API を準備します。

### リソースの作成
IBM Cloud のダッシュボード、もしくはリソース・リストにある `「リソースの作成」ボタン`をクリックします。
![](./img/200.png)

検索テキストボックスに `visual` と入力すると、**[Visual Recognition]** の候補が表示されるのでクリックします。
![](./img/210.png)

サービスの設定画面で **ライト 無料** となっていることを確認して、`「作成」ボタン`をクリックします。
![](./img/220.png)

### API情報表示
作成された API の詳細画面が表示されるので、左メニュの `「管理」` をクリックします。
![](./img/230.png)



## Node-REDへVisual Recognition組み込み
Node-RED と Watson API を連携させる方法はいくつかあります。

- API 鍵による連携
- Cloud Foundry アプリケーションの接続

今回は「API 鍵による連携」で Node-RED と Watson API を連携します。

### API鍵の組み込み
Watson API の管理画面から `資格情報` の `「API 鍵」` をコピーします。
![](./img/240.png)

Node-RED フローエディタを表示し、ワークスペースに配置してある`[visual recognition]`ノードをダブルクリックします。   
**API key** を入力する欄があるので、貼り付けます。
![](./img/250.png)

### EndPoint-URLの組み込み
Watson API の管理画面から `資格情報` の `「URL」` をコピーします。
![](./img/260.png)

Node-RED フローエディタを表示し、ワークスペースに配置してある`[visual recognition]`ノードをダブルクリックします。  
**Service Endpoint** を入力する欄があるので、貼り付けます。
![](./img/270.png)

## Node-REDでWatson Visual Recognition体験
Node-RED と Visual Recognition の連携は完了したので、Watson の機能を試してみます。

### 画像認識結果の言語設定
引き続き `[Visual Recognition]`ノードの設定画面で下記設定をします。

- **Language** を `English`

![](./img/280.png)

### デプロイ
いままで設置してきた フローやコーディングを確定して、実際に動作するようにします。画面上部の `「デプロイ」`ボタンをクリックします。

![](./img/300.png)

Node-RED のサイドバー(右側エリア)上部のデバッグアイコンをクリックし、デバッグサイドバーを表示します。
![](./img/310.png)

## 実行
ワークスペースに配置してある `camera` ノードの左側のボタンをクリックして、フローを実行します。  
するとパソコンのカメラが撮影した写真が `image`ノード下部に表示されます。また、デバッグサイドバーに**画像認識の結果**が出力されます。  
下の例では、「バナナ」を撮影して画像認識させた結果、**banana**、**fruit** などの文字列が表示されていることが確認できます。  
**score** は、Watson の回答の自信度を表しています。

![](./img/330.png)

カエルのぬいぐるみを撮影して画像認識させたところ、**headress** などのご認識されました。これは人の目には「カエル」と認識できても、一般的な「カエル」の特徴が無い為と思われます。

![](./img/320.png)


## 終わりに
ノードを接続するだけで、カメラの制御と写真のプレビュー、そして画像認識が簡単に試せることがわかりました。
画像をスマホから送ったり、別のアプリから送信できるようにすることも可能なので、自作アプリやサービスにも気軽に画像認識機能を組み込むことができます。  
試してみてはいかがでしょうかっ！

