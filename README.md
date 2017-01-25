# Webとクルマのハッカソン2017 開発環境 ReadMe

## 1. W3C Vehicle API について
* W3C Automotive And Web Platform Business Group [link](https://www.w3.org/community/autowebplatform/)
    * 2013年1月活動開始。Vehicle APIについては2014年12月にFinal Business Group Reportとしてドラフトを公開。

* W3C Automotive Working Group [link](https://www.w3.org/auto/wg/)
    * Vehicle APIの仕様策定をBusiness Groupから引き継ぎ、作業継続中。
    * 当初は Business Groupから引き継いだ JavaScript API形式の Vehicle API を推進していたが、2016年に API のアーキテクチャを見直し、現在は WebSocket によるサービス形式の新仕様の Vehicle API の定義を推進している。

### 本ハッカソンで使用するAPI
* Spec document
    * Vehicle Information Access API [link](https://www.w3.org/TR/vehicle-information-api/)
    * Vehicle Data Interfaces [link](https://www.w3.org/TR/vehicle-data/)
* コード例:[link](https://www.w3.org/TR/vehicle-information-api/#introduction)
* 一般的なHTML5 API と同じくブラウザ組み込みのJavaScript APIとして定義されている

### [参考]策定中の最新仕様
* Spec document
    * Vehicle Information Service Specification [link](https://www.w3.org/TR/vehicle-information-service/)
    * Vehicle Signal Client Specification (not ready)
* コード例:[link](https://www.w3.org/TR/vehicle-information-service/#introduction)
* 新APIは車両に搭載したローカルサーバからWebSocketでJSONフォーマットの車両データを受け取り利用するシステム構成となっておりブラウザによるサポートが必要とされない

### 注)最新仕様はまだ定義中の部分があるため、本ハッカソンでは前バージョンのAPIを使用します

## 2.ハッカソン開発環境について

対象車両: TOYOTA Prius 3rd generation<br>
TOYOTA Priusでのテスト走行により走行データを収集しました<br>
<img src="./doc/images/prius_side.jpg" width="400px">

#### システム構成図

<img src="./doc/images/hackathon_arch.png" width="800px">

#### A. ハッカソンサーバー（クラウドサーバー）
ハッカソンサーバーは走行データ（及び、その他センサー系データ）を格納し、Control-UIからのリクエストにより走行データ（及びセンサー系データ）を動画同期させて配信する。

#### B. Control-UI
ハッカソンサーバーを利用するためのUI。利用したい走行パターンを選択すると、走行動画と走行データが同期した状態で再生される。

##### 操作手順

1. [Control-UI](http://52.193.60.25:3000/cluster/controlindex.html?json=20151221-asakusa-skytree.json) を開く
2. `RoomID` テキストボックスにユニークな文字列を入れ、`Connect`をクリック 
3. クライアントアプリケーションを起動しておく(下記ApplicationSampleを参照)
4. Control-uiのドロップダウンリストから走行パターンを選択する
5. 動画エリア内の再生ボタンをクリック
6. 動画が再生すると共に、走行データが動画と同期して配信されControl-UI上のメーターが動作する
7. 同時にクライアントアプリケーションにも走行データが配信され、VehicleAPIによるデータ取得が実行される

注)RoomIDに'aaa'などの単純な文字列を使用すると、他の参加者のRoomIDと重複して動作が不正になる可能性があります
メールアドレスなどユニークさが保証される文字列を使用してください

#### C. クライアントアプリケーション（ハッカソン参加者開発アプリ）
ハッカソン参加者の皆様が開発するWebアプリケーションです。<br>ハッカソンSDKで利用可能となる W3C Vehicle APIを使用することで、自動車の走行情報（及び、走行中のセンサー系データ）を利用できます。<br>
クライアントアプリケーションの作り方はApplicationSampleをご参照ください。<br>Control-UIとの連携動作に必要な最低限のコードで記述されています。

##### ApplicationSample

* [vehicleAll](https://github.com/access-company/AutoWeb-Hackathon/blob/master/ApplicationSample/vehicleSpeed.html)
* [sensorAll](https://github.com/access-company/AutoWeb-Hackathon/blob/master/ApplicationSample/vehicleSpeed.html)
* [location](./ApplicationSample/location.html)

##### 使用手順
1. ApplicationSampleのコードをgithubから取得し参加者の実行環境に配置する
2. ApplicationSampleのコード中のscriptタグで参照するJavaScriptのサーバーIPを使用するサーバーのIP（Control-UIのIP)と合わせておく
3. ApplicationSampleのコード中のRoomID値をControl-UIに設定するRoomIDと合わせておく
4. ApplicationSampleをChromeブラウザで起動して Control-UIからの走行データ配信を待つ
5. Control-UIで走行パターンを選択、再生を行うと、AppliationSampleにも走行データが配信されアプリケーションが動作する

#### D. MapTool
各走行パターンのデータを、地図とグラフ表示で一覧するツール。利用したいデータの変化(例えば急加速、急減速など)が含まれる走行パターンを探すことができる。
##### 操作手順
1. [MapTool](http://52.193.60.25:3000/MapTool/osm_mapping.html) を開く
2. 画面上のドロップダウンリストから走行パターンを選択する
3. 走行パターンのデータが、地図上へのマッピングとグラフにより表示される

#### E. その他

##### 走行パターンリスト
* 利用可能な走行パターン（経路、発生イベント)については下記リストをご参照ください。<br>
また各走行パターンのデータの詳細についてはマップツールでご確認ください。<br>
    * 走行パターンリスト(2015.11～2016.1:センサー系データなし) [link](./doc/files/course_list.pdf)
    * 走行パターンリスト(2016.11～2017.1) [link](./doc/files/course_list.pdf)

##### 走行データのダウンロード利用
* 走行データファイル全体をローカルにダウンロードして利用することも可能です。<br>
走行データは本githubの以下パスに配置してあり、リポジトリのクローンにより取得できます。<br>
    * TODO: link追加

##### Vehicle API利用コード例

get()

    navigator.vehicle.vehicleSpeed.get().then(`
      `function (vehicleSpeed) {console.log("value:"+vehicleSpeed.speed);},`
      `function (error) {console.log("There was an error");}`
    );`

subscribe()/unsubscribe()

    var handle = navigator.vehicle.vehicleSpeed.subscribe(
      function (vehicleSpeed) {console.log("value:"+vehicleSpeed.speed);
      navigator.vehicle.vehicleSpeed.unsubscribe(handle);
    });
   
* 各種データの取得方法はApplicationSampleを参照

## 3. データ項目リスト
#### vehicle data

|*interface*|*attribute*|*unit*|*備考*|
|:----------|:----------|:------------|:------------|
|VehicleSpeed|  speed|km/h|車両速度  |
|EngineSpeed|   speed|rpm|エンジン回転数  |
|VehiclePowerModeType| value|- |イグニッションの状態。offとrunningのみ取得可。<br>0:off, 1:running, 2:accessory, 3:accesory2, 4:engine-cranking|
|AccelerationPedalPosition| value|%|  |
|Transmission|  mode|- |グラフ上はGearと表示。取得状況が不安定な場合があります。<br> -1:reverse, 0:neutral, 1:parking, 2:low, 3:drive|
|LightStatus|   head|- |0:off, 1:on|
||  brake|- |0:off, 1:on|
||  parking|- |0:off, 1:on<br>取得状況が不安定な場合があります|
|Fuel|  level|%|燃料残量|
||  instantConsumption|g/sec |燃料消費量 |
|Acceleration|  x|G |左右方向加速度（座標系説明参照）  |
||  y|G |進行方向加速度（座標系説明参照）  |
||  z|G |鉛直方向加速度（座標系説明参照）<br>重力により静止時 +1G が加わる  |
|Gyro|  x(pitch)|degree/sec |角速度(座標系説明参照) |
||  y(roll)|degree/sec |角速度(座標系説明参照)  |
||  z(yaw)|degree/sec |角速度(座標系説明参照) |
|SteeringWheel|angle|degree|left: plus, right:minus   |
|BrakeOperation|    brakePedalDepressed|0/1| 0:off, 1:on    |
|Odometer|  distanceTotal|km |総走行距離  |
|Door|  status|- |front-right, front-leftのみ。0:open, 1:close|
|Seat|  seatbelt|- |front-rightのみ。0:open, 1:fastened|
|ParkingBrake|  status|- |0:off, 1:on|
|Gps|   latitude|degree|緯度|
||  longitude|degree |経度  |
||  altitude|m|精度に制限があり参考値となります|
||  heading|degree|north:0, East:990, South:180, West:270    |
||  speed|km/h|GPS情報から計算した移動速度  |

#### SDTECH vital data

|*interface*|*unit/value*|*備考*|
|:----------|:------------|:------------|
|Heartrate|bpm|心拍数/分  |
|EmotionCluster|0-25|精神的高揚度(気分の盛り上がり)。0 - 25で評価<br>心拍数、体の動き(加速度)、音声情報をアルゴリズム評価して算出  |

#### JINS MEME raw data
JINS MEME搭載のセンサーの生データ

|*interface*|*unit/value*|*備考*|
|:----------|:------------|:------------|
|EyeMoveUp|1-3|目の動き(上向き)。1,2,3で評価。0は信号なし  |
|EyeMoveDown|1-3|目の動き(上向き)。1,2,3で評価。0は信号なし  |
|EyeMoveRight|1-3|目の動き(上向き)。1,2,3で評価。0は信号なし  |
|EyeMoveLeft|1-3|目の動き(上向き)。1,2,3で評価。0は信号なし  |
|blinkSpeed|micro volt|まばたきの強さ  |
|blinkStrength|ms|閉眼時間  |
|Meme-roll|degree|頭の角度:横方向(座標系説明参照)  |
|Meme-pitch|degree|頭の角度:正面方向(座標系説明参照)  |
|Meme-yaw|degree|頭の角度:水平方向(座標系説明参照)  |
|Meme-accX|1/16G|頭にかかる加速度(座標系説明参照)  |
|Meme-accY|1/16G|頭にかかる加速度(座標系説明参照)  |
|Meme-accZ|1/16G|頭にかかる加速度(座標系説明参照)<br>重力により静止状態で Meme-accZ=-16 となる  |

#### JINS MEME processed data
JINS MEME raw dataをアルゴリズム処理して算出した情報

| *interface* | *unit* | *備考* |
|:----------|:------------|:------------|
|Head-tilt|degree|頭の傾き<br>raw dataをアルゴリズム処理して算出  |
|Awakeness|0-100|覚醒値。100-55:通常, 55-40:やや低い, 40-0:低い |
|Attentiveness|0-100|注意力指数。100-60:通常, 60-40:やや低い, 40-0:低い |

#### 座標系について
車両データとJINS MEMEデータには、x/y/z加速度、ロール/ピッチ/ヨーという3軸データがありますが、車両とJINS MEMEでは座標系が異なりますのでご注意ください。

### 車両データ座標系
<img src="./doc/images/vehicle_axis1.png" width="600px">

<img src="./doc/images/vehicle_axis2.png" width="600px">

### JINS MEME座標系
<img src="./doc/images/meme_axis.png" width="600px">

## 4. マッシュアップAPI

ご協賛をいただいた各社のWebAPIを以下にご紹介します。

* エスディーテック社
    * ドライバーの心拍数、盛り上がり度

* JINS-meme
    * 頭部の加速度（x,y,z）、頭部の回転（roll,pitch,yaw）
まばたき(強さ、閉眼時間)、視線移動(上下左右を3段階)
覚醒度(100点満点、連続値)、注意力(100点満点、連続値)
視線位置(右、中央、左の3段階)

* 昭文社
    * MappleAPI（地図情報、観光情報）

* YuMake
    * 天気情報API（今日明日天気予報、週間天気予報、時系列天気予報、日の出日の入り、注意報警報、地震情報）

* IncrementP
    * インターネット地図配信サービス「Map Fan API」

* KDDIウェブコミュニケーションズ
    * Twilio API

## 5. QAサポート

* 開催日の質問にはチャットアプリSlackにより対応いたします。<br>
  [slack](https://vehicleapi2017.slack.com/)

## 6. 注意事項

* 作業用ブラウザには Google Chrome をご使用ください。
* ハッカソンサーバーは2系統用意してあります。一方の動作が不安定な場合は、もう一方を試してください。
    * 1stサーバ:
        * http://52.193.60.25:3000/cluster/controlindex.html?json=20151221-nihonbashi-ginza.json
        * http://52.193.60.25:3000/MapTool/osm_mapping.html
    * 2ndサーバ:
        * http://13.112.91.95:3000/cluster/controlindex.html?json=20151221-nihonbashi-ginza.json
        * http://13.112.91.95:3000/MapTool/osm_mapping.html
* ハッカソンサーバー、Control-UIほかハッカソン開発環境は、期間中であれば会場外のインターネットからも利用可能です。

* instantConsumptionの単位はg/sとなっており、0 ～ 2000g/secといった値を取ります。
ただし、燃料噴射は瞬間的なもので1秒など連続するものではなく、2000g/secという値を取ったとしても、
1秒で2kgの燃料を消費するわけではありません。

* Altitudeの値は誤差が大きいため参考値となります。

* VehiclePowerModeTypeとは、車両のイグニッションの状態を表します。
今回の場合は'Running'以外の取得はできません。
(Engien Crankingはイグニッションキーを回してセルモーターがエンジンを起動している状態です)

* 走行パターンの中には、走行データ、センサーデータが、先頭部分、途中または末端が欠けているものがありますのでご注意ください。データの欠けはMapToolで確認することができます。
  (動画より先に走行データの再生が終了するとControl-UIのメーターの動作が止まりますが、動画の再生はそのまま継続されます)
    * 01.浅草寺雷門→言問橋
    * 11.塩浜→東京タワー(Theta)

* 以下については事情により走行中の動画の撮影ができなかったため、代替の動画が再生されます。
    * 28.高津→等々力アリーナ（成人式）→246号→飯田橋
    * 65.三角池→木戸池
    * 66.前山ゲレンデ→蓮池

* 3DカメラThetaの動画はControl-UI上のYoutubeウインドウでマウス操作により視点変更が可能です。ただし、確認した限りではUbuntu12.04上のGoogle ChromeではYoutubeの3D動画機能が正しく動作しませんでした。

