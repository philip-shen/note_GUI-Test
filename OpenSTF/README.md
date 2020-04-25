[1. Purpose](#1-purpose)  

[2. OpenSTF (Smartphone Test Farm) on Windows Subsystem for Linux](#2-openstf-smartphone-test-farm-on-windows-subsystem-for-linux)     
[2.1. Environment](#21-environment)     
[2.2. Windows Subsystem for Linux (Ubuntu) Installation](#22-windows-subsystem-for-linux-ubuntu-installation)     
[2.3. RethinkDB Installation](#23-rethinkdb-installation)     
[2.4. STF Related Application and STF stuffs Installation](#24-stf-related-application-and-stf-stuffs-installation)     
[2.5. STF Execution](#25-stf-execution)     
[2.6. Hand On STF](#26-hand-on-stf)     
[2.7. Mobile Devices Operation on STF](#27-mobile-devices-operation-on-stf)         
[2.7.1. Mobile Devices could not Recongnized](#271-mobile-devices-could-not-recongnized)         
[2.7.2. Sony Smart Phone connection with NB](#272-sony-smart-phone-connection-with-nb)         
[2.7.3. HTC Smart Phone connection with NB](#273-htc-smart-phone-connection-with-nb)           
[2.7.3.1. Procedures](#2731-procedures)    
[2.7.3.2. adb Command List](#2732-adb-command-list)   
[2.8. Unable to connect to 127.0.0.1:28015](#28-unable-to-connect-to-12700128015)     
[2.9. How to connect remote rethinkdb ?](#29-how-to-connect-remote-rethinkdb-)     
[2.10. How do I connect to my local STF from another computer](#210-how-do-i-connect-to-my-local-stf-from-another-computer)    
[2.11. Ubuntu 16.04 環境構築](#211-ubuntu-1604-環境構築)     
[Error During Installation Ubuntu Failed at the jpeg-turbo@0.4.0](#error-during-installation-ubuntu-failed-at-the-jpeg-turbo@040)  

[2.12. Chromeを入れる](#212-chromeを入れる)     
[2.13. 起動](#213-起動)  
[2.14. Login](#214-login)  
[2.15. Restart](#215-restart)  

[3. Node and NPM](#3-node-and-npm)    
[4. Troubleshooting](#4-troubleshooting)  
[5. Reference](#5-reference)   
[6. h1 size](#6-h1-size)    
    [5.1. h2 size](#51-h2-size)        
    [5.1.1. h3 size](#511-h3-size)            
        [5.1.1.1. h4 size](#5111-h4-size)                
        [5.1.1.1.1. h5 size](#51111-h5-size)

# 1. Purpose
Take note of OpenSTF (Smartphone Test Farm) Test stuffs  


# 2. OpenSTF (Smartphone Test Farm) on Windows Subsystem for Linux  
[Windows Subsystem for Linux で OpenSTF (Smartphone Test Farm) を動かす Jul 11, 2019](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc)  
## 2.1. Environment    
[試した環境](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc#%E8%A9%A6%E3%81%97%E3%81%9F%E7%92%B0%E5%A2%83)  
```
Windows 10 Home 1903
Linux には Ubuntu 18.04.2 LTS (Bionic Beaver) を使用
```
## 2.2. Windows Subsystem for Linux (Ubuntu) Installation    
![alt tag]()  
![alt tag]()  

Or WSL2  
```
Microsoft Windows [Version 10.0.19608.1006]
Linux には Ubuntu 18.04.2 LTS
```
[03. WSL2 Installation on Win 10](https://github.com/philip-shen/note_Linux/tree/master/WSL(WindowsSubsystemLinux))  

## 2.3. RethinkDB Installation    

![alt tag](https://i.imgur.com/AxDq6jM.jpg)  

## 2.4. STF Related Application and STF stuffs Installation    
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

## 2.5. STF Execution    
さきほど書いた手順で RethinkDB を起動しておきます。
起動しておかないと STF が正しく起動しません。  

RethinkDB を起動した状態で Ubuntu上 で stf local を実行します。（どの階層で実行してもOK）
下のような実行結果になるはず。  
```
stf local
```

## 2.6. Hand On STF  
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

## 2.7. Mobile Devices Operation on STF   
### 2.7.1. Mobile Devices could not Recongnized  
[端末が認識されない場合](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc#%E7%AB%AF%E6%9C%AB%E3%81%8C%E8%AA%8D%E8%AD%98%E3%81%95%E3%82%8C%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88)  
[[Android] デバッグモードで接続した端末がadb devicesで認識されない場合の対処法 2018.08.15](https://webbibouroku.com/Blog/Article/adb-interface-driver-update)  

### 2.7.2. Sony Smart Phone connection with NB  
[[基礎教學]安裝ADB和Fastboot驅動，適用於所有Sony手機。Mar 13, 2016](https://www.mobile01.com/topicdetail.php?f=569&t=4736079)  

```
A.安裝Fastboot和ADB驅動，以及讓電腦正確判讀到手機內部空間。
B.Fastboot解鎖與注意事項。
```
```
A-2.上面做完後，去手機設定-->安全性，不明來源要打勾；然後一樣回到設定-->關於平板電腦,連續點擊最下面的版本號碼，直到通知你已經是開發人員，回到上一頁就會發現開發人員選項，右上角點開啟、OEM解鎖打勾，USB偵錯打勾，就像我下圖一模一樣這樣。(如果你沒有OEM解鎖打勾就忽略沒關係。)
```
![alt tag](https://i.imgur.com/2GiVbJk.jpg)  

### 2.7.3. HTC Smart Phone connection with NB  
[利用ADB讓Android7.0雙視窗可浮動(無須Root) - HTC論壇 Dec 23, 2017](https://community.htc.com/tw/chat.php?mod=viewthread&tid=85008)  
```
使用手機：hTC 10
電腦系統：Win 10
```
#### 2.7.3.1. Procedures    
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

#### 2.7.3.2. adb Command List    

[如何在電腦上使用ADB指令操作Android手機 ... - 阿旺師磨書坊 Dec 16, 2018 Updated](http://wangwangtc.blogspot.com/2015/03/adbandroid.html)  
```
adb devices (顯示目前有多少個模擬器正在執行)
adb devices -l  (顯示目前有多少個模擬器正在執行, 更詳細的資料)
```
```
E:\adb>adb devices
List of devices attached
470f02f1 device


E:\adb>adb devices -l
List of devices attached
470f02f1 device product:geefhd_open_tw model:LG_E988
```

```
adb push  <local> <remote>          - 複製電腦端檔案/資料夾到 SD 卡
adb push J:\software\software\android\download\apk\line.apk  /storage/external_SD/apk

adb push i:\software\software\android\download\apk\gps_map\naviking\NaviKingMap201505\NaviKingMap  /storage/external_SD/Android/data/com.kingwaytek/files/NaviKingMap

adb pull  <remote> [<local>]        - 從SD 卡複製檔案/資料夾到 電腦端
adb pull /sdcard/cwm_6.0.4.7.recovery.img  .\cwm_6.0.4.7.recovery.img

adb reboot (手機重新開機)
adb reboot recovery (手機重新開機，進入recovery 模式)
adb reboot fastboot   (手機重新開機，進入fastboot 模式)


adb shell (進入 Android 系統指令列模式)

常用的shell 指令(跟 linux相同)

dmesg  - 查看 Android Linux Kernel 運作訊息
 ls - 顯示檔案目錄
 cd - 進入目錄
rm - 刪除檔案
 mv - 移動檔案
mkdir - 產生目錄
 rmdir - 刪除目錄
su - 使用 root權限
pwd - 顯示目前路徑名稱
dd - 複製與轉換檔案  copy and convert a file
        if : input file  (輸入檔案)    of :  output file (輸出檔案)
dd if=/sdcard/cwm6.0.4.7.recovery.img of=/dev/block/platform/msm_sdcc.1/by-name/recovery
```
```
E:\adb>adb shell shell@geefhd:/ $ su
root@geefhd:/ # pwd
root@geefhd:/ # cd data 
root@geefhd:/data #
```



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


## 2.8. Unable to connect to 127.0.0.1:28015 
[FTL Error No hosts left to try when run `stf local --public-ip xxxx` Nov 13, 2017](https://github.com/openstf/stf/issues/752)  
```
You probably don’t have rethinkdb running. Read the README.
```
![alt tag](https://i.imgur.com/Hmz1yRz.jpg)

## 2.9. How to connect remote rethinkdb ?  
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


## 2.10. How do I connect to my local STF from another computer  
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


*Error Msg*
![alt tag](https://i.imgur.com/P6oMzlv.jpg)  
[n now works on windows 10 (using windows subsystem for Jun 11, 2018](https://github.com/tj/n/issues/511)  
[cant run react server on Windows Subsystem for Linux (ubuntu) Nov 3, 2019](https://stackoverflow.com/questions/58681101/cant-run-react-server-on-windows-subsystem-for-linux-ubuntu)  


[Ubuntu 16.04にOpenSTF環境構築 Sep 29, 2017](https://qiita.com/tanaka512/items/4792931fe08b08441dc7)  
## 2.11. Ubuntu 16.04 環境構築  
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
```

```
#nodejs
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

sudo npm install -g bower karma gulp
```

```
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
```

```
sudo ldconfig
```

```
#OpenSTF
cd
mkdir openstf
cd openstf/
git clone https://github.com/openstf/stf.git
cd stf
sudo npm install -g stf --unsafe-perm
```

### Error During Installation Ubuntu Failed at the jpeg-turbo@0.4.0  
[Error During installation ubuntu Failed at the jpeg-turbo@0.4.0#1066 Jun 24, 2019](https://github.com/openstf/stf/issues/1066)  
```
Refer to: #950 , #1015
Use nvm to install node 8.x, 
and npm install -g stf without sudo, everything goes well.
Node 10.x seems to be not compatible yet.
```
[ nvm-sh /nvm](https://github.com/nvm-sh/nvm#git-install)
[nodejs 10.+ version cannot npm install stf successfull #1015](https://github.com/openstf/stf/issues/1015)
[npm install -g stf gives errors #950](https://github.com/openstf/stf/issues/950)  
```
Don’t use sudo. Use nvm instead and you won’t need it.

Also, use node 8.x for now.
```
[nvmを使ったNode.jsのバージョン管理 Jan 22, 2020](https://qiita.com/kei10/items/688229d5266ed613e157)  
1. nvm をインストール  
```
$ git clone https://github.com/nvm-sh/nvm.git .nvm
```
2. nvm を使えるようにする  
```
$ source ~/.nvm/nvm.sh
```

```
$ nvm --version
```
3. ~/.bashrcに追加  
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

```
source .bashrc
```

3. Node.jsをインストール  
```
$ nvm ls-remote
```
```
$ nvm install <version>
```
```
$ node -v
$ npm -v
```
![alt tag](https://i.imgur.com/akRvlcM.jpg)   
![alt tag](https://i.imgur.com/7IkEUtQ.jpg)   
![alt tag](https://i.imgur.com/NknXBTB.jpg)   

![alt tag](https://i.imgur.com/xW7qZDE.jpg)  
![alt tag](https://i.imgur.com/WNaNWbj.jpg)  


[nvm(Node Version Manager)を使ってNode.jsをインストールする手順 Dec 22, 2017](https://qiita.com/ffggss/items/94f1c4c5d311db2ec71a)  
[nvm-windows 導入 Oct 11, 2018](https://qiita.com/rapando/items/6e9d891789b9a652c318)  
```
１．適当なフォルダに解凍する
D:\tools\nvm-noinstall

２．環境変数を設定する
NVM_HOME
D:\tools\nvm-noinstall

NVM_SYMLINK
D:\tools\nodejs

PATHに追加する
;%NVM_HOME%;%NVM_SYMLINK%

３．settings.txtを設置する
D:\tools\nvm-noinstall
settings.txt

settings.txtの中身
root: D:\tools\nvm-noinstall
path: D:\tools\nvm_nodejs
arch: 64
proxy: none

example
https://github.com/coreybutler/nvm-windows/blob/master/examples/settings.txt

４．コマンドプロンプトを起動して nvm が動くか確認
C:\Users\uchi>nvm
Running version 1.1.7.
Usage:
以下省略。。。。

５．nodeをインストール
コマンドプロンプトより
C:\Users\uchi>nvm install latest
Downloading node.js version 10.11.0 (64-bit)...
Complete
Creating D:\tools\nvm-noinstall\temp
Downloading npm version 6.4.1... Complete
Installing npm v6.4.1...
Installation complete. If you want to use this version, type
nvm use 10.11.0

６．使用するnode.jsのバージョンを設定する。
この時点では10.11.0しかないため10.11.0を指定する
C:\Users\uchi>nvm use 10.11.0
Now using node v10.11.0 (64-bit)
※nvm入れる前にnode.jsをインストールしている場合は
　先にアンインストールする。
　もしくは％PATH%からC:\Program Files\nodejsを削除する

７．nodeのバージョンを確認する
C:\Users\uchi>node -v
v10.11.0

８．現在nvmで管理しているnodeのリストを確認する。
C:\Users\uchi>nvm list
10.11.0

９．node.jsの別バージョンをインストールする
C:\Users\uchi>nvm install 8.12.0
Downloading node.js version 8.12.0 (64-bit)...
Complete
Creating D:\tools\nvm-noinstall\temp
Downloading npm version 6.4.1... Complete
Installing npm v6.4.1...
Installation complete. If you want to use this version, type
nvm use 8.12.0

１０．node.jsのバージョンを切り替える
念のためnode.jsのリストを確認
C:\Users\uchi>nvm list
* 10.11.0 (Currently using 64-bit executable)
8.12.0
バージョンを切り替える
C:\Users\uchi>nvm use 8.12.0
Now using node v8.12.0 (64-bit)

C:\Users\uchi>nvm list
10.11.0
* 8.12.0 (Currently using 64-bit executable)

C:\Users\uchi>node -v
v8.12.0

以上。
```
[nvm + Node.js + npmのインストール Nov 19, 2016](https://qiita.com/sansaisoba/items/242a8ba95bf70ba179d3)  


[Debian/UbuntuでNode.jsをインストールする(nvm) Nov 11, 2014](https://qiita.com/tamurashingo@github/items/6348863668e1e3fd70c9)  


## 2.12. Chromeを入れる  
```
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

## 2.13. 起動  
```
#rethinkdb起動
rethinkdb

#OpenSTF起動
stf local
```

## 2.14. Login  
[STF credentials · Issue #811 · openstf/stf · GitHub Feb 21, 2018](https://github.com/openstf/stf/issues/811)  
```
It does not ask for a password. 
It asks for your name and email address. You can enter anything you want.
```
![alt tag](https://i.imgur.com/ZpGt2Bt.jpg)  

![alt tag](https://i.imgur.com/S39C7vp.jpg)  


[openstf /setup-examples ](https://github.com/openstf/setup-examples)  
```
STF Setup Examples using Vagrant and Docker 

Smartphone Test Farm is a great tool to create on-premise device farm. 
And also, it is very easy to get started with it using it's stf local feature. 
But the problem is, this feature was developed for development purpose. 
Users are not supposed to use it in production. 
It is okay to use it for a small farm of 10 ~ 15 devices with one host machine. 
But to scale you will have to deploy it over a cluster instead.

This project will provide various setup examples of STF in production environment. 
I will be using Vagrant with VirtualBox provider to create virtual cluster for demonstration.
```

## 2.15. Restart  
```
# source Downloads/.nvm/nvm.sh

# nvm ls

# node -v

# export RETHINKDB_PORT_28015_TCP='tcp://192.168.1.108:28015'

# .nvm/versions/node/v8.17.0/bin/stf doctor

# adb devices -l

# .nvm/versions/node/v8.17.0/bin/stf local
```
![alt tag](https://i.imgur.com/P954qQq.jpg)  

![alt tag](https://i.imgur.com/EJIzZO5.jpg)  

![alt tag](https://i.imgur.com/6HS1giw.jpg)  

## 2.16. OpenSTF on Docker    

[OpenSTF+Dockerで社内Android端末管理システムをMac上に構築する Sep 04, 2018; updated at Feb 13, 2020](https://qiita.com/KazaKago/items/26db0f68ba224eb094d3)  


[Androidのリモート操作/管理ツール「OpenSTF」をDocker for Windowsで使う Sep 16, 2019](https://www.scriptlife.jp/contents/programming/2019/09/16/openstf-docker-for-windows/#OculusQuest)  
```
試した環境は次の通り。

    Windows 10 Professional
    Docker for Windows
    adbがインストールされている

　Windows10 Homeだと、Docker Toolkitを使うことになってしまうので、この方法だけではダメかもしれません。

　adbによる接続を前提としたツールなので、adbはインストール済みとします。
```

```
docker-compose.yml

version: "3"
services:
  db:
    image: rethinkdb
    command: rethinkdb --bind all
    volumes:
      - ./db_data:/data
  stf:
    image: openstf/stf
    ports:
      - 7100:7100
      - 7110:7110
      - 7400-7679:7400-7679
    links:
      - db
    environment:
      - RETHINKDB_PORT_28015_TCP=tcp://db:28015
      - RETHINKDB_ENV_DATABASE=stf
    command: stf local --allow-remote --public-ip host.docker.internal --adb-host host.docker.internal --provider-max-port 7679    
```



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

# 3. Node and NPM  
[從零開始: 使用NPM套件- 進擊的Front End's - Medium Sep 4, 2017](https://medium.com/html-test/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B-%E4%BD%BF%E7%94%A8npm%E5%A5%97%E4%BB%B6-317beefdf182)  
```
安裝 NVM (node version manager)
```

[NPM是什麼？了解Node Package Manager套件管理機制 ... Oct 10, 2019](https://tw.alphacamp.co/blog/npm-node-package-manager)  
[npm常用指令教學-Node.js套件管理工具| ucamc Dec 29, 2018](https://www.ucamc.com/e-learning/computer-skills/324-npm%E5%B8%B8%E7%94%A8%E6%8C%87%E4%BB%A4%E6%95%99%E5%AD%B8-node-js%E5%A5%97%E4%BB%B6%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7)  
[史上最強套件管理 - NPM ， npm init 與 npm install (Day11) 2017-12-14](https://ithelp.ithome.com.tw/articles/10191682)  

[npm 最佳實作的小技巧| DEVLOG of andyyou Oct 4, 2016](https://andyyou.github.io/2016/10/04/node-best-practice/)  
[Setting up a Node development environment - 學習該如何開發 ... Mar 18, 2019](https://developer.mozilla.org/zh-TW/docs/Learn/Server-side/Express_Nodejs/development_environment)  


# 4. Troubleshooting


# 5. Reference


* []() 
![alt tag]()  

# 6. h1 size

## 5.1. h2 size

### 5.1.1. h3 size

#### 5.1.1.1. h4 size

##### 5.1.1.1.1. h5 size

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
