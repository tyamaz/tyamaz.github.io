---
title: Galaxy Z Flip 3 で Chrome をリモートデバッグする
aliases:
  - Galaxy Z Flip 3 で Chrome をリモートデバッグする
tags:
  - d/2022/04/27
  - n/PGM/Android/12
---

環境

- Android12
- Windows10



Galaxy に限らず Android なら大体同じだと思われる。

[Galaxy_Z_Flip_3_を開発者モードにする](Galaxy_Z_Flip_3_を開発者モードにする.md) の方法で開発者モードにする。


設定の開発者メニューから USBデバッグを ON にする。
Z Flip を一旦再起動する。※ 必要無い可能性もあるがやっておいたほうがよい。

Windows10側に Android 端末のドライバを導入する。
ドライバが、別の Android 端末向けのドライバとバッティングしている可能性もあり、関連するドライバをアンインストールする必要がある可能性もある。

自分の場合は SuperDisplay という Android 端末を外部ディスプレイペンタブレットにするアプリのドライバとバッティングした。

ドライバは各社から提供されているのでそれを入れる。ドライバへのリンクは [OEM USB ドライバのインストール  \|  Android デベロッパー  \|  Android Developers](https://developer.android.com/studio/run/oem-usb) このページの下部にまとまっている。


一応、なくなることも考慮して全コピーしておく

- Acer [https://www.acer.com/worldwide/support/](https://www.acer.com/worldwide/support/)
- Alcatel Mobile [https://www.alcatelmobile.com/support/](https://www.alcatelmobile.com/support/)
- ASUS [https://www.asus.com/jp/support/Download-Center/](https://www.asus.com/support/Download-Center/)
- Blackberry [https://swdownloads.blackberry.com/Downloads/entry.do?code=4EE0932F46276313B51570F46266A608](https://swdownloads.blackberry.com/Downloads/entry.do?code=4EE0932F46276313B51570F46266A608)
- Dell [https://support.dell.com/support/downloads/index.aspx?c=us&cs=19&l=en&s=dhs&~ck=anavml](https://support.dell.com/support/downloads/index.aspx?c=us&cs=19&l=en&s=dhs&~ck=anavml)
- 富士通 [http://www.fmworld.net/product/phone/sp/android/develop/](http://www.fmworld.net/product/phone/sp/android/develop/)
- HTC [https://www.htc.com/support](https://www.htc.com/support)
- HUAWEI [https://consumer.huawei.com/en/support/index.htm](https://consumer.huawei.com/en/support/index.htm)
- Intel [https://www.intel.com/software/android](https://www.intel.com/software/android)
- 京セラ [https://kyoceramobile.com/support/drivers/](https://kyoceramobile.com/support/drivers/)
- Lenovo [https://support.lenovo.com/us/en/GlobalProductSelector](https://support.lenovo.com/us/en/GlobalProductSelector)
- LGE [https://www.lg.com/us/support/software-firmware](https://www.lg.com/us/support/software-firmware)
- Motorola [https://motorola-global-portal.custhelp.com/app/answers/detail/a_id/88481/](https://motorola-global-portal.custhelp.com/app/answers/detail/a_id/88481)
- MTK [http://online.mediatek.com/Public%20Documents/MTK_Android_USB_Driver.zip](http://online.mediatek.com/Public%20Documents/MTK_Android_USB_Driver.zip)（ZIP ダウンロード）
- Samsung [https://developer.samsung.com/galaxy/others/android-usb-driver-for-windows](https://developer.samsung.com/galaxy/others/android-usb-driver-for-windows)
- シャープ [http://k-tai.sharp.co.jp/support/](http://k-tai.sharp.co.jp/support/)
- Sony Mobile Communications [https://developer.sonymobile.com/downloads/drivers/](https://developer.sonymobile.com/downloads/drivers/)
- 東芝 [https://support.toshiba.com/sscontent?docId=4001814](https://support.toshiba.com/sscontent?docId=4001814)
- Xiaomi [https://web.vip.miui.com/page/info/mio/mio/detail?postId=18464849&app_version=dev.20051](https://web.vip.miui.com/page/info/mio/mio/detail?postId=18464849&app_version=dev.20051)
- ZTE [http://support.zte.com.cn/support/news/NewsDetail.aspx?newsId=1000442](http://support.zte.com.cn/support/news/NewsDetail.aspx?newsId=1000442)

Z Flip なので Samsung のページからダウンロードする。
別に機種毎のドライバは無いっぽくて、インストーラーが1個あるだけなのでそれでインストールすればよい。

一応 Windows も再起動したほうがよいかも。

そして USB ケーブルで PC と Z Flip を繋ぐ。
イマドキなので Type-C になるがこのケーブルをやや選ぶっぽい。
全部入りのぶっといケーブルではうまくいかず、USB2.0規格の Type-C ならうまくいったりと、このへんよくわからない。
問題があるならケーブルを疑うとよい。

接続すると、Android 側に許可を求めるダイアログが出るので、許可する。


PC側の Google Chrome を起動して、

```
chrome://inspect/#devices
```

にアクセスする。

Android 側の Chrome を起動してデバッグしたいページを開く。

数秒放置すると、PC 側の画面(`chrome://inspect/#devices`)に Android 側ページの一覧が出てくる。
その一覧にある `inspect` というリンクを押すと別ウィンドウが開いて通常のPCの DevTool で Android 側の画面を操作できる。



















