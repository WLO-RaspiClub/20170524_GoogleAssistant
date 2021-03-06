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

## インストールの手順

### Google Cloud Platformの設定
- https://cloud.google.com/?hl=ja
    - GCPトップ画面 <br> ![GCP画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/GCP-0.png)
- Google Accountでログインする
    - GCPログイン画面 <br> ![GCPログイン画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/GCPlogin1.png)
    - GCPパスワード画面 <br> ![GCPログイン画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/GCPlogin2.png)
    - GCP登録画面 <br> ![GCP登録画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/GCPjoin1.png)
    - お客様情報の入力（住所とかカード番号とか） 画像なし <br> 
    - GCP Welcome画面 <br> ![GCP Welcome画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/GCPconsole1.png)
- プロジェクトを作成
    - メニュー上部のプロジェクト選択メニュー <br> ![プロジェクト１](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project1.png)
    - 新規プロジェクト(+ボタン) <br> ![新規プロジェクト](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project2.png)
    - プロジェクト名入力 <br> ![プロジェクト名入力](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project3.png)
    - プロジェクト作成確認 <br> ![プロジェクト作成確認](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project4.png)
    - API ManagerダッシュボードでAPIを有効にするを押下 <br> ![ダッシュボード](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project5.png)
    - Google Assistant APIを検索して有効にする <br> ![ダッシュボード](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project6.png) <br> ![ダッシュボード](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/project7.png)
- 認証情報を作成
    - API Managerダッシュボードで認証情報 <br> ![ダッシュボード](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/auth1.png)
    - Oauth同意画面でメールアドレスとサービス名を入力 <br> ![Oauth同意画面](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/auth2.png)
    - Oauth2.0クライアントキー作成 <br> ![Oauthクライアントキー作成](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/auth3.png)
    - JSONをダウンロード <br> ![クレデンシャルダウンロード](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/auth4.png)
        - ``` /home/pi/client_secret_client-id.json ``` に保存しておく
- Google Accountのアクティビティコントロールを有効に
    - https://myaccount.google.com/activitycontrols?pli=1
    - 「端末情報」と「音声アクティビティ」をONにする <br> ![アクティビティ](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/myactiv.png)

### Raspberry PiにGoogle Assistant SDKをインストールする
- Python3のvenv環境を作成

```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install python3-dev python3-venv
$ sudo apt-get install portaudio19-dev libffi-dev libssl-dev
$ python3 -m venv env
$ env/bin/python -m pip install --upgrade pip setuptools
```

- Python3のvenv環境に入る
```
$ source env/bin/activate 
```

- Google Assistant SDKをバイナリインストール(Raspberry Pi 3専用、google-assistant-demoのみ導入)
```
(env) $ python -m pip install --upgrade https://github.com/googlesamples/assistant-sdk-python/releases/download/0.3.0/google_assistant_library-0.0.2-py2.py3-none-linux_armv7l.whl
```

-  Google Assistant SDKをインストール(Raspberry Pi 3以外でもできるが時間かかる、googlesamples-assistant-audiotest等も導入できる)
```
(env) $ python -m pip install google-assistant-sdk[samples]
```

- Oauth認証ツールをインストールする
``` 
(env) $ python -m pip install --upgrade google-auth-oauthlib[tool]
```

- 認証情報を設定する
```
(env) $ google-oauthlib-tool --client-secrets /home/pi/client_secret_client-id.json --scope https://www.googleapis.com/auth/assistant-sdk-prototype --save --headless
```
![コンソール](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/oauth_taken.png) <br>
(長いURLが表示されるので、ブラウザでアクセスする）

- ログイン画面表示されるのでログインする <br> ![OAuthログイン](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/Oauth1.png)

- トークンが表示される <br> ![認証確認](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/Oauth2.png)

![コンソール](https://github.com/WLO-RaspiClub/20170524_GoogleAssistant/raw/master/img/oauth_taken2.png) <br>
(Enter the authorization code にトークンを入力すると、 /home/pi/.config/google-oauthlib-tools/credentials.json に保存される)

- (参考) Python3のvenv環境からでる
```
(env) $ deactivate 
```

### Raspberry Piの音声環境設定
- デバイスの接続状況を確認する
- 再生デバイス
```
 $ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: Set [USB Headphone Set], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  Subdevices: 8/8
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
  Subdevice #7: subdevice #7
card 1: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

- 録音デバイス
```
$ arecord -l
**** List of CAPTURE Hardware Devices ****
card 0: Set [USB Headphone Set], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```


- ~/.asoundrc をvi等で編集して、デバイスを指定する
    - ```pcm "hw:0,0" ```　←card:subdeviceの順で番号を指定する

```
pcm.!default {
  type asym
  capture.pcm "mic"
  playback.pcm "speaker"
}
pcm.mic {
  type plug
  slave {
    pcm "hw:0,0"
  }
}
pcm.speaker {
  type plug
  slave {
    pcm "hw:0,0"
  }
}
```

- 確認
    - ``` $ arecord --format=S16_LE --duration=5 --rate=16k --file-type=raw out.raw ``` で5秒録音
    - ``` $ aplay --format=S16_LE --rate=16k out.raw ``` で再生
    - Googleのツールで確認（バイナリインストール時はできません）
```
(env) $ googlesamples-assistant-audiotest
INFO:root:Starting audio test.
INFO:root:Recording samples. (ここでマイクにむかって話す)
INFO:root:Finished recording.
INFO:root:Playing back samples.
INFO:root:Finished playback. (ここで聞こえたら合格)
INFO:root:audio test completed.
```

### google-assistant-demoで「OK Google」
- ツールでテスト（バイナリインストール時）
```
(env) $ google-assistant-demo
ON_MUTED_CHANGED:
  {'is_muted': False}
ON_START_FINISHED  (ここで「OK Google」と言う

ON_CONVERSATION_TURN_STARTED (OK Googleが反応すると表示。このあと「What time is it now」と言う)
ON_END_OF_UTTERANCE
ON_RECOGNIZING_SPEECH_FINISHED:
  {'text': 'what time is it now'}　(聞き取った文字を表示) 
ON_RESPONDING_STARTED:
  {'is_error_response': False}   (ここでThe time is xxxと聞こえる)
ON_RESPONDING_FINISHED
ON_CONVERSATION_TURN_FINISHED:
  {'with_follow_on_turn': False}

```

- ツール紹介 ``` env/bin ``` にインストールされます
    -（バイナリインストール時にはgooglesamples-*はインストールされません）
    - googlesamples-assistant-hotword
    - googlesamples-assistant-pushtotalk

## IFTTT連携設定


    
