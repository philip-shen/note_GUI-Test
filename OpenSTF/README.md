# Purpose
Take note of OpenSTF (Smartphone Test Farm) Test stuffs  

# Table of Contents  
[]()  
[]()  

# 
[Windows Subsystem for Linux で OpenSTF (Smartphone Test Farm) を動かす Jul 11, 2019](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc)  
## 試した環境  
[試した環境](https://qiita.com/PikachuPunch/items/1c0c469df8aa8f4339dc#%E8%A9%A6%E3%81%97%E3%81%9F%E7%92%B0%E5%A2%83)  
```
 Windows 10 Home 1903
 Linux には Ubuntu 18.04.2 LTS (Bionic Beaver) を使用
```
## Windows Subsystem for Linux (Ubuntu) の インストール  
![alt tag]()  
![alt tag]()  

## RethinkDB のインストール  

![alt tag](https://i.imgur.com/AxDq6jM.jpg)  

## STF に必要な他のアプリと、STF 本体のインストールする  
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

## STF の実行  
さきほど書いた手順で RethinkDB を起動しておきます。
起動しておかないと STF が正しく起動しません。  

RethinkDB を起動した状態で Ubuntu上 で stf local を実行します。（どの階層で実行してもOK）
下のような実行結果になるはず。  
```
stf local
```

## Hand On STF  
```
$ export RETHINKDB_PORT_28015_TCP='tcp://192.168.1.217:28015'
$ stf local --allow-remote
```
![alt tag](https://i.imgur.com/TJAC7jM.jpg)  
![alt tag](https://i.imgur.com/doq0A1T.jpg)  

![alt tag](https://i.imgur.com/mrC3F1f.jpg)   



## Unable to connect to 127.0.0.1:28015 
[FTL Error No hosts left to try when run `stf local --public-ip xxxx` Nov 13, 2017](https://github.com/openstf/stf/issues/752)  
```
You probably don’t have rethinkdb running. Read the README.
```
![alt tag](https://i.imgur.com/Hmz1yRz.jpg)

## How to connect remote rethinkdb ?  
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


## How do I connect to my local STF from another computer  
[How do I connect to my local STF from another computer ](https://github.com/openstf/stf/issues/536)  
```
1. Either rethinkdb is not running or your machine is not able to connect with rethinkdb. Check it first.

2. You are using wrong command. Instead of stf local 10.1.X.X 10.1.X.X you need to run stf local --public-ip <IP_ADDRESS_OF_CURRENT_MACHINE>. After this, from another machine(should be in same network) access http://IP_ADDRESS_OF_MACHINE_WHERE_STF_IS_RUNNING:7100
```

[mac系统搭建手工测试openstf - CodeToSurvive的个人博客 Nov 2, 2016](https://codetosurvive1.github.io/posts/mac-openstf-environment.html)  


[Ubuntu18にOpenSTFを入れてみた Jul 26, 2018](http://yakisaba.hatenablog.jp/entry/2018/07/26/103620)  


[Ubuntu 16.04にOpenSTF環境構築 Sep 29, 2017](https://qiita.com/tanaka512/items/4792931fe08b08441dc7)  
## 環境構築  
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

## Chromeを入れる  
```
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

## 起動  
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
