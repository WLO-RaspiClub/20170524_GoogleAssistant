## Goal
- Raspberry Piにつないだマイクとスピーカーで「OK Google」する
- (optional) IFTTTでLINEとかと連携する

## 準備するもの
- Raspberry Pi 3
    - 3以降でもいけると思うけれど、途中のツールのインストールに時間がかかると思われる (ためしてない）
    - Raspbian 2017-04-10 (LiteでもたぶんOK)
- マイクとスピーカ
    - スピーカは本体のオーディオジャックでもいいけど調整が必要なので、USBドングルでマイクスピーカ端子があるのが楽。（音質はよくないけど）
    - Bluetoothだと確実にハマるので有線がいい
- Google Account
    - Google Developer登録時にクレジットカードを登録する必要がある。
    - このために新たにgmail アカウントを取得するのがいいかも？

## 参照
- 基本は
    - https://developers.google.com/assistant/sdk/prototype/getting-started-pi-python/
- 「Google Assistant Raspberry Pi」とかで検索するとでてくるが、大抵はgoogle-assistant-sdkのバージョンが古く手順が異なる。
    - google-assistant-sdk が0.3.0以降に対応している手順かどうか確認
    
## Google Cloud Platformの設定
- https://cloud.google.com/?hl=ja
    - ![GCP画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/GCP-0.png)
- Google Accountでログインする
    - ![GCPログイン画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/GCPlogin1.png)
    - ![GCPログイン画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/GCPlogin2.png)
    - ![GCP登録画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/GCPjoin1.png)
    - お客様情報の入力（住所とかカード番号とか）
    - ![GCP Welcome画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/GCPconsole1.png)
- プロジェクトを作成
    - プロジェクト名入力
    - API Managerダッシュボード
    - Google Assistant APIを有効に
- 認証情報を作成
    - Oauth同意画面で入力
    - Oauth2.0クライアントキー作成
    - JSONをダウンロード
- Google Accountのアクティビティコントロールを有効に
    - https://myaccount.google.com/activitycontrols?pli=1
    - 「端末情報」と「音声アクティビティ」をONにする

## Raspberry PiにGoogle Assistant SDKをインストールする
- Python3のvenv環境を作成

```
$ sudo apt-get install python3-dev python3-venv
$ sudo apt-get install portaudio19-dev libffi-dev libssl-dev
$ python3 -m venv env
$ env/bin/pip install setuptools --upgrade
```

- Python3のvenv環境に入る
```
$ source env/bin/activate 
```

- Python3のvenv環境からでる
```
(env)$ deactivate 
```



## Raspberry Piの音声環境設定


## google-assistant-demoで「OK Google」

## IFTTT連携設定


    
