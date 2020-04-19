
[1. Purpose](#1-purpose)  
[2. Table of Contents](#2-table-of-contents)  
[3. OpenSTF (Smartphone Test Farm) on Windows Subsystem for Linux](#3-openstf-smartphone-test-farm-on-windows-subsystem-for-linux)auto    
    - [3.1. Environment](#31-environment)auto    
    - [3.2. Windows Subsystem for Linux (Ubuntu) Installation](#32-windows-subsystem-for-linux-ubuntu-installation)auto   
    - [3.3. RethinkDB Installation](#33-rethinkdb-installation)auto    
    - [3.4. STF Related Application and STF stuffs Installation](#34-stf-related-application-and-stf-stuffs-installation)auto    
    - [3.5. STF Execution](#35-stf-execution)auto    
    - [3.6. Hand On STF](#36-hand-on-stf)auto    
    - [3.7. Mobile Devices Operation on STF](#37-mobile-devices-operation-on-stf)auto        
        - [3.7.1. Mobile Devices could not Recongnized](#371-mobile-devices-could-not-recongnized)auto        
        - [3.7.2. Sony Smart Phone connection with NB](#372-sony-smart-phone-connection-with-nb)auto        
        - [3.7.3. HTC Smart Phone connection with NB](#373-htc-smart-phone-connection-with-nb)auto            
            - [3.7.3.1. Procedures](#3731-procedures)auto    
    - [3.8. Unable to connect to 127.0.0.1:28015](#38-unable-to-connect-to-12700128015)auto    
    - [3.9. How to connect remote rethinkdb ?](#39-how-to-connect-remote-rethinkdb-)auto    
    - [3.10. How do I connect to my local STF from another computer](#310-how-do-i-connect-to-my-local-stf-from-another-computer)auto    
    - [3.11. 環境構築](#311-環境構築)auto    
    - [3.12. Chromeを入れる](#312-chromeを入れる)auto    
    - [3.13. 起動](#313-起動)auto
[4. Troubleshooting](#4-troubleshooting)auto- 
[5. Reference](#5-reference)auto- 
[6. h1 size](#6-h1-size)auto    
    - [6.1. h2 size](#61-h2-size)        
        - [6.1.1. h3 size](#611-h3-size)
        - [6.1.1.1. h4 size](#6111-h4-size)
        - [6.1.1.1.1. h5 size](#61111-h5-size)

# 1. Purpose
Take note of OpenSTF (Smartphone Test Farm) Test stuffs  

# 2. Table of Contents  
[OpenSTF (Smartphone Test Farm) on Windows Subsystem for Linux](#openstf-smartphone-test-farm-on-windows-subsystem-for-linux)  
[]()  


# 3. OpenSTF (Smartphone Test Farm) on Windows Subsystem for Linux  
[Windows Subsystem for Linux で OpenSTF (Smartphone Test Farm) を動かす Jul 11, 2019](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc)  
## 3.1. Environment    
[試した環境](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc#%E8%A9%A6%E3%81%97%E3%81%9F%E7%92%B0%E5%A2%83)  
```
Windows 10 Home 1903
Linux には Ubuntu 18.04.2 LTS (Bionic Beaver) を使用
```
## 3.2. Windows Subsystem for Linux (Ubuntu) Installation    
![alt tag]()  
![alt tag]()  

Or WSL2  
```
Microsoft Windows [Version 10.0.19608.1006]
Linux には Ubuntu 18.04.2 LTS
```
[03. WSL2 Installation on Win 10](https://github.com/philip-shen/note_Linux/tree/master/WSL(WindowsSubsystemLinux))  

## 3.3. RethinkDB Installation    

![alt tag](https://i.imgur.com/AxDq6jM.jpg)  

## 3.4. STF Related Application and STF stuffs Installation    
```
sudo apt-get update
```

```
sudo apt-get -y install npm
```
![alt tag](https://i.imgur.com/UoWGFAk.jpg)  


```
sudo apt-get -y install git android-tools-adb python autoconf automake libtool build-essential ninja-build libzmq3-dev libprotobuf-dev graphicsmagick yasm stow
```
![alt tag](https://i.imgur.com/LAVesDP.jpg)  

```
#ZeroMQ
mkdir ~/Downloads
cd ~/Downloads
wget http://download.zeromq.org/zeromq-4.1.2.tar.gz
tar -zxvf zeromq-4.1.2.tar.gz
cd zeromq-4.1.2
./configure --without-libsodium --prefix=/usr/local/stow/zeromq-4.1.2
make
sudo make install
cd /usr/local/stow
sudo stow -vv zeromq-4.1.2
```
![alt tag](https://i.imgur.com/sBTx0Gk.jpg)  
![alt tag](https://i.imgur.com/zSGOGMN.jpg)  
![alt tag](https://i.imgur.com/OoTzkSN.jpg)  
![alt tag](https://i.imgur.com/JHBa3r7.jpg)  

```
#Google protobuf 10分程度かかる
cd ~/Downloads
git clone https://github.com/google/protobuf.git
cd protobuf
./autogen.sh
./configure --prefix=/usr/local/stow/protobuf-`git rev-parse --short HEAD`
make
sudo make install
cd /usr/local/stow
sudo stow -vv protobuf-*
```
![alt tag](https://i.imgur.com/X6c17yw.jpg)  
![alt tag](https://i.imgur.com/KWC1BS7.jpg)  
![alt tag](https://i.imgur.com/LuRJpiM.jpg)  

```
#OpenSTF
cd
mkdir openstf
cd openstf/
git clone https://github.com/openstf/stf.git
cd stf
sudo npm install -g stf --unsafe-perm
```
![alt tag](https://i.imgur.com/Bv0vI0U.jpg)  
![alt tag](https://i.imgur.com/0dJyDab.jpg)  

## 3.5. STF Execution    
さきほど書いた手順で RethinkDB を起動しておきます。
起動しておかないと STF が正しく起動しません。  

RethinkDB を起動した状態で Ubuntu上 で stf local を実行します。（どの階層で実行してもOK）
下のような実行結果になるはず。  
```
stf local
```

## 3.6. Hand On STF  
```
$ export RETHINKDB_PORT_28015_TCP='tcp://192.168.1.217:28015'
$ stf local --allow-remote
```
![alt tag](https://i.imgur.com/TJAC7jM.jpg)  
![alt tag](https://i.imgur.com/doq0A1T.jpg)  

![alt tag](https://i.imgur.com/mrC3F1f.jpg)   

```
$ export RETHINKDB_PORT_28015_TCP='tcp://192.168.1.217:28015'
$ stf local --allow-remote --public-ip xx.xx.xx.xx
```
![alt tag](https://i.imgur.com/EPT9gfW.jpg)  

## 3.7. Mobile Devices Operation on STF   
### 3.7.1. Mobile Devices could not Recongnized  
[端末が認識されない場合](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc#%E7%AB%AF%E6%9C%AB%E3%81%8C%E8%AA%8D%E8%AD%98%E3%81%95%E3%82%8C%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88)  
[[Android] デバッグモードで接続した端末がadb devicesで認識されない場合の対処法 2018.08.15](https://webbibouroku.com/Blog/Article/adb-interface-driver-update)  

### 3.7.2. Sony Smart Phone connection with NB  
[[基礎教學]安裝ADB和Fastboot驅動，適用於所有Sony手機。Mar 13, 2016](https://www.mobile01.com/topicdetail.php?f=569&t=4736079)  

```
A.安裝Fastboot和ADB驅動，以及讓電腦正確判讀到手機內部空間。
B.Fastboot解鎖與注意事項。
```
```
A-2.上面做完後，去手機設定-->安全性，不明來源要打勾；然後一樣回到設定-->關於平板電腦,連續點擊最下面的版本號碼，直到通知你已經是開發人員，回到上一頁就會發現開發人員選項，右上角點開啟、OEM解鎖打勾，USB偵錯打勾，就像我下圖一模一樣這樣。(如果你沒有OEM解鎖打勾就忽略沒關係。)
```
![alt tag](https://i.imgur.com/2GiVbJk.jpg)  

### 3.7.3. HTC Smart Phone connection with NB  
[利用ADB讓Android7.0雙視窗可浮動(無須Root) - HTC論壇 Dec 23, 2017](https://community.htc.com/tw/chat.php?mod=viewthread&tid=85008)  
```
使用手機：hTC 10
電腦系統：Win 10
```
#### 3.7.3.1. Procedures    
```
1.設定 >> 開發人員選項 >> USB 偵錯>> 打勾
* 如何開啟開發人員選項？設定 >> 關於  >> 軟體資訊 >> 更多 >> 建置號碼(點5下即可開啟)
```

```

2.下載ADB工具 >> 右鍵解壓縮 >> 
在ADB工具資料夾裡的空白處Shift+右鍵 >> 在此處開啟命令視窗(Win10: PowerShell)

* 下載連結：連結為Google雲端硬碟 下載點
* 確認手機有連接到電腦可在命令視窗中輸入 adb devices 後按Enter，下方文字為已正確連接
```
![alt tag](https://i.imgur.com/zN3cm5T.jpg)  

```
3.輸入 adb shell settings put global enable_freeform_support 1
```

```
4.手機會跳出視窗請點同意/允許，確認已正確輸入，如未跳出可直接重新開機(有跳出也請重開)
```

[Android studio如何連接HTC Desire 12手機debug - 程式分享 Aug 26, 2019](http://phappy1122.blogspot.com/2019/08/android-studiohtc-desire-12debug.html)  
```
(a)
點選系統 > 關於手機。
點選關於手機。
點選"版本號碼七次以上"，直到出現您現在已經是開發人員的訊息。
```

```
(b)
若自己寫的程式若放入Htc時無法安裝原因有三:
  (1)設定/安全性/不明的來源(設為允許)
  (2)設定/google/安全性/google play安全防護(點選)/按右上角的齒輪，關閉掃描裝置中的安全
                                                                                                                                                        威脅 
  (3)關閉藍光濾波器(點選最上menu，即可看到，若無就表沒有)
```

```
(c)
HTC與android連結，首先須adb連結，先將手機用usb線與電腦連線，此時會在"控制台/裝置管理員"，
內會出現adb?的圖示，此時需安裝adb驅動程式，安裝方式如下網頁

http://adbdriver.com/documentation/how-to-install-adb-driver-on-windows-8-10-x64.html
https://www.laird.tw/2017/03/usb-adb-interface-driver-error.html

安裝完後開啟 View->Tool Windows->Device File Explorer 即可看到Htc裝置，若有顯示權限要求，請將手機的"開發者選項/USB debug"重新開關即可。
如此在執行run時，即會看到htc裝置，此時已可以使用android studio進行debug trace。
```


[如何在電腦上使用ADB指令操作Android手機 ... - 阿旺師磨書坊 Dec 16, 2018 Updated](http://wangwangtc.blogspot.com/2015/03/adbandroid.html)  

[【新手看了也會】hTC 官方解鎖輕鬆搞定@ 耶魯熊の軟硬兼施 Aug 26, 2019](https://lbear.pixnet.net/blog/post/58440430)  
[[Android] debug不用線，用ADB連接3G/wifi手機@ 清新下午茶 May 12, 2019](https://j796160836.pixnet.net/blog/post/29108155)  
```
4.  按Win key + R，在執行的視窗中打入 cmd
打入指令 (綠色的為指令，黑色的部分為說明)
C:\
cd C:\Program Files\Android\android-sdk\platform-tools
意思是切換資料夾到剛剛找的路徑
adb tcpip 5555 
意思是用tcpip連線，連接埠號5555做Debug伺服器
```

```
5.  然後就可以脫離USB連線了
在同一個地方再打入像是
adb connect 192.168.1.3:5555 
中間換成你手機的IP位址
意思是讓電腦使用網路連線到你的手機
```


## 3.8. Unable to connect to 127.0.0.1:28015 
[FTL Error No hosts left to try when run `stf local --public-ip xxxx` Nov 13, 2017](https://github.com/openstf/stf/issues/752)  
```
You probably don’t have rethinkdb running. Read the README.
```
![alt tag](https://i.imgur.com/Hmz1yRz.jpg)

## 3.9. How to connect remote rethinkdb ?  
[How to connect remote rethinkdb ? Oct 14, 2015](https://github.com/openstf/stf/issues/122)  
```
With environment variables: https://github.com/openstf/stf/blob/master/lib/db/index.js#L16
```

```
Yes, thanks. I just find that.

Another small question, if i change the nodejs code under '/usr/local/lib/node_modules/stf/', and restart the STF, do the change code work ?

Thanks.
```

```
If it's server-side code, then yes.
```

```
Thanks for your help.

I try to start remote rethinkdb like this:

rethinkdb --http-port xxxx --bind xxx.xxx.xxx.xxx --driver-port 9901

and the rethinkdb response:

Server ready, "liufuxinlucky365" ea74d40e-6ff0-46ee-a90e-0d9db54ad4ed

and i start stf like this:

    export RETHINKDB_PORT_28015_TCP='tcp://xxx.xxx.xxx.xxx:9901'
    stf local --allow-remote

response:

    INF/db 2690 [*] Connecting to xxx.xxx.xxx.xxx:9901
    FTL/db 2690 [*] Could not connect to xxx.xxx.xxx.xxx:9901, operation timed out.

but it seems that stf doesn't connect successfully. Have you ever try this feature ?
```


## 3.10. How do I connect to my local STF from another computer  
[How do I connect to my local STF from another computer ](https://github.com/openstf/stf/issues/536)  
```
1. Either rethinkdb is not running or your machine is not able to connect with rethinkdb. Check it first.

2. You are using wrong command. Instead of stf local 10.1.X.X 10.1.X.X you need to run stf local --public-ip <IP_ADDRESS_OF_CURRENT_MACHINE>. After this, from another machine(should be in same network) access http://IP_ADDRESS_OF_MACHINE_WHERE_STF_IS_RUNNING:7100
```

[OpenSTF を Mac に入れたら 簡単にAndroid端末をリモート操作できた話 Aug 03, 2019](https://qiita.com/enuesutea/items/bbf2e88fa79f44aee2e0)  
[インストールしたアプリを消さない設定で OpenSTF を実行](https://qiita.com/enuesutea/items/bbf2e88fa79f44aee2e0#%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%97%E3%81%9F%E3%82%A2%E3%83%97%E3%83%AA%E3%82%92%E6%B6%88%E3%81%95%E3%81%AA%E3%81%84%E8%A8%AD%E5%AE%9A%E3%81%A7-openstf-%E3%82%92%E5%AE%9F%E8%A1%8C)
```
# 以下のIPアドレスは例で、実際はインストールしたMacのIPアドレスを指定
$ stf local --no-cleanup --public-ip 192.168.1.10
```
[スクリーンキャプチャの画質を落として転送量を減らす](https://qiita.com/enuesutea/items/bbf2e88fa79f44aee2e0#%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%AD%E3%83%A3%E3%83%97%E3%83%81%E3%83%A3%E3%81%AE%E7%94%BB%E8%B3%AA%E3%82%92%E8%90%BD%E3%81%A8%E3%81%97%E3%81%A6%E8%BB%A2%E9%80%81%E9%87%8F%E3%82%92%E6%B8%9B%E3%82%89%E3%81%99)  
```
# 以下のように環境変数を設定してから、STF を起動する
$ export SCREEN_JPEG_QUALITY=30
```

 
[mac系统搭建手工测试openstf - CodeToSurvive的个人博客 Nov 2, 2016](https://codetosurvive1.github.io/posts/mac-openstf-environment.html)  


[Ubuntu18にOpenSTFを入れてみた Jul 26, 2018](http://yakisaba.hatenablog.jp/entry/2018/07/26/103620)  


[Ubuntu 16.04にOpenSTF環境構築 Sep 29, 2017](https://qiita.com/tanaka512/items/4792931fe08b08441dc7)  
## 3.11. 環境構築  
```
# rethinkdb
source /etc/lsb-release && echo "deb http://download.rethinkdb.com/apt $DISTRIB_CODENAME main" | sudo tee /etc/apt/sources.list.d/rethinkdb.list
wget -qO- https://download.rethinkdb.com/apt/pubkey.gpg | sudo apt-key add -

#色々
sudo apt-get update
sudo apt-get install git
sudo apt-get install npm
sudo apt-get install rethinkdb
sudo apt-get install android-tools-adb
sudo apt-get install python
sudo apt-get install autoconf
sudo apt-get install automake
sudo apt-get install libtool
sudo apt-get install build-essential
sudo apt-get install ninja-build
sudo apt-get install libzmq3-dev
sudo apt-get install libprotobuf-dev
sudo apt-get install graphicsmagick
sudo apt-get install yasm
sudo apt-get install stow

#nodejs
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

sudo npm install -g bower karma gulp

mkdir ~/Downloads

#ZeroMQ
cd ~/Downloads
wget http://download.zeromq.org/zeromq-4.1.2.tar.gz
tar -zxvf zeromq-4.1.2.tar.gz
cd zeromq-4.1.2
./configure --without-libsodium --prefix=/usr/local/stow/zeromq-4.1.2
make
sudo make install
cd /usr/local/stow
sudo stow -vv zeromq-4.1.2

#Google protobuf
cd ~/Downloads
git clone https://github.com/google/protobuf.git
cd protobuf
./autogen.sh
./configure --prefix=/usr/local/stow/protobuf-`git rev-parse --short HEAD`
make
sudo make install
cd /usr/local/stow
sudo stow -vv protobuf-*

sudo ldconfig

#OpenSTF
cd
mkdir openstf
cd openstf/
git clone https://github.com/openstf/stf.git
cd stf
sudo npm install -g stf --unsafe-perm
```

## 3.12. Chromeを入れる  
```
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

## 3.13. 起動  
```
#rethinkdb起動
rethinkdb

#OpenSTF起動
stf local
```

[OpenSTF+Dockerで社内Android端末管理システムをMac上に構築する Sep 04, 2018](https://qiita.com/KazaKago/items/26db0f68ba224eb094d3)  

[OSSなリモートスマホサービス（STF）が素敵すぎる Jan 30, 2017](https://qiita.com/tabbyz/items/5f6cec37e1d525a8e4d5)  

[OpenSTFとkintoneでモバイル端末を管理する話  2018-12-20](https://blog.cybozu.io/entry/2018/12/20/110000)  
```
こんにちは。品質保証部の園です。 
モバイル端末、何台持っていますか? 管理面倒ですよね!!
積年の問題を解決すべく、OpenSTFとkintoneを組み合わせて、
検証用のモバイル端末数十機を管理する仕組みを作りました。
はまりどころ含めてご紹介します。
```

[OpenSTFでAndroidのCIを2倍早くする  2016-08-15](https://techlife.cookpad.com/entry/2016/08/15/200000)  

[Android App 自動化測試：OPEN-STF環境搭建 2017-10-24](https://kknews.cc/digital/65xqxlp.html)  

# 4. Troubleshooting


# 5. Reference


* []() 
![alt tag]()  

# 6. h1 size

## 6.1. h2 size

### 6.1.1. h3 size

#### 6.1.1.1. h4 size

##### 6.1.1.1.1. h5 size

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
