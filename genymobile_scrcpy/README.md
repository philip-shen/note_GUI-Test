Table of Contents
=================

   * [Purpose](#purpose)
   * [問題点](#問題点)
   * [画面消したままでも安定した！あとWindowsのタッチパネルに対応したっぽい(v1.10)](#画面消したままでも安定したあとwindowsのタッチパネルに対応したっぽいv110)
   * [バッチ記述例](#バッチ記述例)
   * [noconsoleバージョンで書いた起動スクリプト(v1.17)](#noconsoleバージョンで書いた起動スクリプトv117)
   * [動作がぬるいのをオプションで改善してみる（※参考バッチ）](#動作がぬるいのをオプションで改善してみる参考バッチ)
   * [Troubleshooting](#troubleshooting)
   * [Reference](#reference)
   * [h1 size](#h1-size)
      * [h2 size](#h2-size)
         * [h3 size](#h3-size)
            * [h4 size](#h4-size)
               * [h5 size](#h5-size)
   * [Table of Contents](#table-of-contents)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)


# Purpose
Take note of genymobile/scrcpy

# 問題点  
[PCでAndroid端末を操作したい(wifi, non root) updated at 2021-01-16](https://qiita.com/ooba1192/items/79c3aae8f48cd663aefd)  
[問題点](https://qiita.com/ooba1192/items/79c3aae8f48cd663aefd#%E5%95%8F%E9%A1%8C%E7%82%B9)

```
    Android5以上の端末が必要。最低動作環境。

    Android端末の再起動のたびにUSBで直接接続してadb tcpip xxxxの操作が必要。
        ただAndroidは基本起動しっぱなし運用だろうし大きな問題ではないのでは。
        root化済み端末だと自動で設定できる → 再起動したあと自動的にadb tcpipを有効化する (root化必要)
    
    音が出ないので別途Android端末から音を引っ張ってくるものが必要(bluetoothヘッドホンとか)。
        公式でページ下の方に以下記述あり
        Linuxで、USB接続が前提だが一応実装はあるみたい
    
    マウスで操作できるがショートカットは覚える必要がある。
    
    片側が有線LANでないと動作遅延が酷い。PC(有線LAN)←→Android(無線LAN)だと快適。PC(無線LAN)←→Android(無線LAN)だと
    
    100~500msくらい遅延する気がする。
        かと思いきや、端末パワー依存っぽい。左記構成でも、貧弱スマホだと遅い。
        Zenpad3 8.0(2016/9発売)は片側無線接続でも快適だが、同じ接続方式でもXperia Z3 Compact(2014/11発売)は動作がぬるい。
    
    Android側に有線LANアダプタが欲しくなるが、アダプタの対応端末が少ない。
```

# 画面消したままでも安定した！あとWindowsのタッチパネルに対応したっぽい(v1.10)  
```
2019/08にリリースされたv1.10を試してみてる途中で気付いたのだけどタッチパネル長押しに対応したっぽいです。
普通にタブレットPCから指で長押しが反応するようになった模様！右クリック反応が起きない！これで捗るね。。
あと画面暗転起動のオプションがv1.9だと不安定だったけどv1.10だと安定してるっぽいです。
一晩置いておいても接続に失敗しない！

追記
暗転起動オプション、リモート接続は問題ないんだけど、、今度は本体が暗転してどうやっても復帰しなくなる気がする。
scrcpyで接続すると動くんだけど、本体直で弄るのが出来なくなるっぽい。
このオプションはもうしばらく様子見再開。
```

```
scrcpy.exe -w
```
 
# バッチ記述例  
```
ポートは何処でもよさげです。公式例では5555。
Android側のIPアドレス固定については、やったほうがいいですが、必須ではないので必要なら検索してください。
使ってるAndroid端末のIPアドレスの調べ方はこのへんで。

2019/06/17 以下のセクションのバッチをすべて盛り込んだ最新のバッチをここに記載する。
#複数端末を同時に操作する
#動作がぬるいのをオプションで改善してみる
DOS窓が邪魔なのでバッチファイルを最小化で起動の情報を参考に組み込んだ。

--bit-rate は1.5Mという指定をしたかったけど小数点解釈してくれないようなので1500Kという表現で代用。
```

```
@echo off
if not "%~0"=="%~dp0.\%~nx0" (
     start /min cmd /c,"%~dp0.\%~nx0" %*
     exit
)
cd /d "C:\Users\xxx\OneDrive\sync\scrcpy"
::
adb connect 192.168.1.5:5555
::scrcpy -s 192.168.1.5 -p 5555 --bit-rate 2M --max-size 1024 --turn-screen-off この行はコメントアウト行
scrcpy -s 192.168.1.5 -p 5555 --bit-rate 1500K --max-size 1024
adb disconnect 192.168.1.5:5555
```

# noconsoleバージョンで書いた起動スクリプト(v1.17)  
[noconsoleバージョンで書いた起動スクリプト(v1.17)](https://qiita.com/ooba1192/items/79c3aae8f48cd663aefd#noconsole%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%81%A7%E6%9B%B8%E3%81%84%E3%81%9F%E8%B5%B7%E5%8B%95%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%97%E3%83%88v117)  
```
CreateObject("Wscript.Shell").Run "cmd /c scrcpy -s 192.168.1.5 -p 5955 --bit-rate 3M --max-size 1344 --turn-screen-off", 0, true
```

# 動作がぬるいのをオプションで改善してみる（※参考バッチ） 
```
scrcpyのドキュメントを見ると、デフォルトだと8M/bで通信しているらしい。
ここを2M程度にすると反応がよくなった。
さらに画面解像度を1024辺りに制限すると更によくなった気がする。
ただし画面は粗い。
Surfaceと上述のぬるいxperiaで使ってみたが、無線LAN←→無線LANでも動作遅延が許容できるレベルに。
```

```
cd /d "C:\Users\xxx\OneDrive\sync\scrcpy"
::
adb connect 192.168.1.5:5555
scrcpy -s 192.168.1.5 -p 5555 --bit-rate 2M --max-size 1024
::pause
adb disconnect 192.168.1.5:5555
```


# Troubleshooting


# Reference


* []()
![alt tag]()

# h1 size

## h2 size

### h3 size

#### h4 size

##### h5 size

*strong*strong  
**strong**strong  

> quote  
> quote

- [ ] checklist1
- [x] checklist2

* 1
* 2
* 3

- 1
- 2
- 3


