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
    
    
