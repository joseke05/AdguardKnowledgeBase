---
title: '制限付きプロファイルが原因の問題'
published: true
taxonomy:
    category:
        - docs
---


制限付きプロファイル（アカウント）のあるAndroid 7以降デバイスの一部（特にSamsung Galaxy S10系端末）で起こる問題です。このようなプロファイルがある場合、**AdGuard**は、VPNを使用する他アプリケーションと同様に、VPNトラフィックの選択的なフィルタリングに対する制限がかかってしまいます。その結果、AdGuardは**ローカルVPNモード**での保護を起動できなくなります。
また、この状況の理由の1つは、デバイスで**デュアルアプリ/デュアルメッセンジャー**を使用していることです。

### 解決方法

現在対処方法は3つございます（多くの方にとって方法①が一番便利かと思いますが）。

### 方法①: *制限付きアカウント*を削除する

多くの端末の場合:
端末設定→詳細設定→複数ユーザー→制限付きプロファイルを削除する


ユーザーアカウント管理については[こちら](https://support.google.com/a/answer/6223444?hl=ja)にてさらにご確認いただけます。
> 制限付きユーザーアカウントが他の機能を通じて作成され、直接削除できない場合があることに注意してください。たとえば、**Samsung**または**LG**デバイスでデュアルメッセンジャーまたはデュアルアプリ機能を使用する場合です。これらのケースで問題を解決する方法を以下に記載いたしました。


#### LGとSamsungデバイス

**LG**または**Samsung**端末には「**デュアルアプリ**」または「デュアルメッセンジャー」機能を使用することにより、制限付きプロファイルが作成され、VPN起動の問題が発生する可能性があります。
この問題を解決するには、その機能を無効にする必要があります。


#### Samsung端末

- 端末設定→高度な設定→デュアルメッセンジャー
- チェックついているアプリをすべてオフにする
- 端末画面オフにして5～10分程度放置する

#### LG端末

- 端末設定を開く
- 「便利な機能」を開く（端末によっては「一般」や他のメニューの場合があります）
- 「デュアルアプリ」を開く
- アプリに対するスイッチをすべてオフにする
- 端末を再起動する

### 方法②: ADB経由でAdGuardに権限を与える

> ※この方法は**AdGuard v3.5 Nightly 6**以降で利用できることに注意してください。古いバージョンを使用している場合は、[こちらから](adguard.com/beta.html)のNightly最新バージョンを入手できます（もしくはAdGuardアプリ内設定で「アップデートチャンネル」を「Nightly」に切り替えてアプリをアップデートできます）。

❶ **開発者モード**をアクティブにし、**USBデバッグ**を有効にします。

- 端末で**設定**アプリを開きます。
- **システム**セクションに移動し（設定メニューの最後の項目）、サブアイテム**端末について**を見つけます。
- 「**ビルド番号**」の行を7回タップします。その後、「**開発者になりました！**」のような通知が表示されます（必要に応じて、デバイスのロック解除コードを入力してください）。
- **設定**→**システム**→**詳細設定**→**開発者向けオプション**→下にスクロールして「**USBデバッグ**」を開く（もしくはオンにする）→警告を注意深く読んでいただいた後、[**USBデバッグを許可する**]ウィンドウでデバッグが有効になっていることを確認します。

※端末によってメニュー項目の名称が多少違ったりする場合がございます。

>  上記に関してまだご不明やお困りな点ございましたら、[こちらで](https://developer.android.com/studio/debug/dev-options?hl=ja)さらに詳しい手順をご確認ください。

❷ADBをインストトールして設定します（方法：[Windows編](https://expnote.com/how-to-install-android-debug-bridge/)、[Mac編](https://child-programmer.com/m-adb/)）。

❸ **USBケーブル**を使用して**ADB**をインストールしたコンピューターまたはラップトップにAndroidデバイスを接続します。

❹ PCで**コマンドライン**を開きます。

- **Windows**を使用している場合は**Cmd.exe**
- **macOS**を使用している場合は**ターミナル**

❺ `adb shell pm grant com.adguard.android android.permission.INTERACT_ACROSS_USERS` というコマンドを入力して**Enter**を押します。これで完了です。


### 方法③: *ローカルHTTPプロキシモード*でAdGuardを使用する（ルート権限が必要になります）

このモードを有効にする手順:
**AdGuard設定**→**ネットワーク**→**フィルタリング方式**（近日中「フィルタリング方法」に変わります）→ **ローカルHTTPプロキシ**を選択

