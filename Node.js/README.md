
Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Purpose](#purpose)
   * [Node.js Test Tool](#nodejs-test-tool)
      * [Installation](#installation)
      * [Sample Code](#sample-code)
   * [Node.JS - 30 天入門學習筆記](#nodejs---30-天入門學習筆記)
   * [Node.js Rookie](#nodejs-rookie)
      * [Node.js Installation on Windows](#nodejs-installation-on-windows)
      * [Environment](#environment)
      * [npm and yarn](#npm-and-yarn)
      * [yarnをインストール](#yarnをインストール)
   * [Node on MacOS/Linux](#node-on-macoslinux)
      * [Installation on Mac](#installation-on-mac)
      * [Installation on Linux](#installation-on-linux)
   * [Node on WSL](#node-on-wsl)
      * [anyenv and nodenv Installation](#anyenv-and-nodenv-installation)
         * [anyenv](#anyenv)
         * [nodenv](#nodenv)
      * [Node.js Installation](#nodejs-installation)
      * [Files Operation on Windwos](#files-operation-on-windwos)
   * [Node.js on Docker](#nodejs-on-docker)
      * [Docker files](#docker-files)
   * [Front End, Test Tool](#front-end-test-tool)
   * [Node.js and iPhone by iBeacon Test](#nodejs-and-iphone-by-ibeacon-test)
   * [Troubleshooting](#troubleshooting)
   * [Reference](#reference)
      * [supertest](#supertest)
   * [h1 size](#h1-size)
      * [h2 size](#h2-size)
         * [h3 size](#h3-size)
            * [h4 size](#h4-size)
               * [h5 size](#h5-size)
   * [Table of Contents](#table-of-contents-1)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)

# Purpose
Take note Test Tools of Node.js  

# Node.js Test Tool  
[Node.js テストツール updated at 2019-06-25](https://qiita.com/ganariya/items/4e41263af84095f26a4b)  

## Installation  
```
$ npm install --save-dev expect
$ npm install --save-dev mocha
$ npm install --save-dev supertest
```
## Sample Code  
```
utils.js

module.exports.add = (a, b) => {
    return a + b;
}

module.exports.square = (x) => {
    return x * x;
}

module.exports.setName = (user, fullName) => {
    var names = fullName.split(' ');
    user.firstName = names[0];
    user.lastName = names[1];
    return user;
}

module.exports.asyncAdd = (a, b, callback) => {
    setTimeout(()=>{
        callback(a+b);
    }, 1000)
}

//コールバックはC++でいうインターフェイスに近い
//定義自体は呼び出し元に記述する
module.exports.asyncSquare = (x, callback) =>{
    setTimeout(()=>{
        callback(x*x);
    }, 1500)
}
```

```
utils.test.js

const expect = require('expect');
const utils = require('./jtils');



//テストに名前をつける
//describeは階層構造にできる
describe('ユーティリティテスト', () => {

    describe('#Not Async', () => {
        it('should add two numbers', () => {
            var result = utils.add(33, 11);

            // if(result !== 44){
            //     throw new Error(`44ののはずが、${res}だった！`);
            // }

            expect(result).toBe(44).toBeA('number');

        });

        it('should square a number!', () => {
            var result = utils.square(10);

            expect(result).toBe(100).toBeA('number');

        })


        it('should expect some values', () => {

            expect({
                name: 'hogehoge',
                age: 25,
                location: 'tokyo'
            }).toInclude({
                name: 'hogehoge',
                age: 25
            })
        })

        it('should be the same name', () => {

            var fullName = 'hoge piyo';
            var names = {
                firstName:'hoge',
                lastName:'piyo'
            }
            var results = utils.setName(names,fullName)

            expect(results).toInclude({
                firstName: 'hoge',
                lastName: 'piyo'
            })

        })

    })

    describe('#Async', () => {
        //コールバック関数に、doneを取ると、doneが実行されるまでチェックを行う
        it('should add two numbers async', (done) =>{

            utils.asyncAdd(20, 30, (sum) => {
                expect(sum).toBe(50).toBeA('number');
                done();
            })

        })



        it('should square a number async', (done) => {

            utils.asyncSquare(10, (sum) => {
                expect(sum).toBe(100).toBeA('number');
                done();
            })

        })

    })



})

```

```
server.js

const express = require('express');

var app = express();


app.get('/', (req, res, next) => {
    res.status(200).send({
        name: 'hogehoge',
        title: 'title'
    })
})


app.get('/users', (req, res, next) => {
    res.status(200).send({
        name: 'hogehoge',
        age: 20
    })
})


app.listen(3000);

module.exports.app = app;  //検査できるようにexportする

```

```
server.test.js

const request = require('supertest');
const expect = require('expect');

var app = require('./server.js').app;


describe('サーバーテスト', () => {

    describe('#GET /', () => {
        it('should return hello world response', (done) => {
            request(app)
                .get('/')
                .expect(200)  //Status
                .expect((res) => {
                    expect(res.body).toInclude({
                        name: 'hogehoge'
                    })
                })
                .end(done);
        })
    })

    describe('#GET /users', () => {
        it('should return a name', (done) => {
            //supertest→request
            //引数はapp(元のmodule.exportから受け取る)
            request(app)
                .get('/users')
                .expect(200)
                .expect((res) => {
                    expect(res.body).toInclude({
                        name:'hogehoge',
                        age: 20
                    })
                })
                .end(done);
        })
    })


})
```


# Node.JS - 30 天入門學習筆記  
[Day30 - 實作 繼續向前](https://ithelp.ithome.com.tw/articles/10188051)  

```
Day1 - Node.js 開發環境準備
Day2 - 使用Sublime 編譯 nodeJS
Day3 - Node.js Modules 介紹及載入
Day4 - 自建模組(Local Modules )與如何使用
Day5 - 關於 module.exports 的兩三事
```
[Day6 - Node Package Manager (NPM) 套件管理](http://ithelp.ithome.com.tw/articles/10185008)  
```
Day7 - Node.js 內建的 Web Server 介紹及使用
Day8 - Node.js 檔案系統
Day9 - node.js 內建除錯
Day10 - Node Inspector
Day11 - Node.js EventEmitter
Day12 - Express.js 簡介及安裝
Day13 - express web application
Day14 - cURL 工具
Day15 - node.js使用靜態檔案服務
Day16 - Node.js 串接 MS-SQL Server
Day17 - MongoDB 安裝設定
Day18 - Node.JS 串接 MongoDB (含CRUD)
Day19 - 樣版引擎及Jade 樣版引擎的安裝與使用
Day20 - Jade樣版與資料庫應用-以MS-SQL為例
Day21 - Jade樣版與資料庫應用-以MongoDB為例
Day22 - Jade樣版的Layout
Day23 - Cookie & Session
Day24 - Cookie在express上的應用—登入實作為例
Day25 - session 在 express 上的應用 – 登入實作為例
Day26 - RESTful 架構開發 與 MVC 設計
Day27 - 實作 TODO List (Model)
Day28 - 實作 TODO List (View)
Day29 - 實作 TODO List (Controller)
Day30 - 實作 繼續向前
跨年彩蛋篇 - 使用雲端mongoDB，以Azure為例。
新年彩蛋篇 - node.js 部署至雲端，以Azure為例
```


# Node.js Rookie  
[本当の初心者のためのNode.js超入門　~環境構築編~ Nov 19, 2019](https://qiita.com/qulylean/items/0ad521885a04a5ebd202)  
## Node.js Installation on Windows  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F350776%2F414cfd41-4573-a0c1-fb67-22b50a9a9758.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=534d0278a4117fc6f3ffa59454172b44)  


[本当の初心者のためのNode.js超入門　~Webサーバー構築編~ Nov 21, 2019](https://qiita.com/qulylean/items/afa2acff6ed963c88798)  
## Environment  
項目 | スペック
------------------------------------ | --------------------------------------------- 
プロセッサ | Intel(R)Xeon
メモリ | 16GB
OS | Windows 10Pro
node.js | v12.13.0
npm | 6.12.0
yarn | 1.19.1

## npm and yarn  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fupload.wikimedia.org%2Fwikipedia%2Fcommons%2Fthumb%2Fd%2Fdb%2FNpm-logo.svg%2F1280px-Npm-logo.svg.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=d77ab1aad3b50de178d7e92576fc38e3)  

```
広く世界中で利用されているnpmですが、以下の2点が問題として指摘されています。

  インストール時にコードを実行することを許可しているため、セキュリティー上の問題がある
  インストールパッケージの速度及び一貫性が不十分
```

## yarnをインストール  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F350776%2Fca71724b-c4ed-ce63-3495-95f833e7e543.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=660a868238b6456ba49600fcd1cf01db)


# Node on MacOS/Linux  
[初心者にオススメのNode.jsインストール方法(MacOS/Linux) Mar 03, 2019](https://qiita.com/jigengineer/items/a7da86e8b4d4e3471656)  

## Installation on Mac  
* Homebrewのインストール
* nodenvのインストール
* Node.jsのインストール

```
1.    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
2.    brew install nodenv
3.   echo "export PATH=~/.nodenv/shims:\$PATH" >> ~/.bash_profile
4.    source ~/.bash_profile
5.    nodenv install 10.15.1
6    nodenv global 10.15.1

確認: node --version -> v10.15.1 と表示されたら成功
```

## Installation on Linux  
[Linuxbrew](https://docs.brew.sh/Homebrew-on-Linux)


# Node on WSL  
[WSLでWindowsにLinux開発環境を構築する Apr 16, 2018](https://qiita.com/taquaki-satwo/items/b379a141492ce8927deb)  

## anyenv and nodenv Installation  
[3. anyenvとnodenvのインストール](https://qiita.com/taquaki-satwo/items/b379a141492ce8927deb#3-anyenv%E3%81%A8nodenv%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)  

### anyenv  
```
git clone https://github.com/riywo/anyenv ~/.anyenv
echo 'export PATH="$HOME/.anyenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(anyenv init -)"' >> ~/.bashrc
exec $SHELL -l
```

### nodenv  
```
anyenv install nodenv
...中略
exec $SHELL -l
```

## Node.js Installation  
```
nodenv install --list
...中略
nodenv install 9.11.1
...中略
nodenv install 8.11.1
...中略
nodenv global 9.11.1
node -v
v9.11.1
```

##  Files Operation on Windwos  
```
cd /mnt/c/Users/<USER_NAME>
```

```
cd /mnt/c/Users/<USER_NAME>
mkdir workspace
cd workspace
nodenv local 8.11.1
node -v
v8.11.1
```

```
echo 'console.log("HELLO WORLD")' > helloWorld.js
node helloWorld
HELLO WORLD
```


# Node.js on Docker  
[初心者こそ、Docker を使おう！ updated at 2019-10-21](https://qiita.com/atoris/items/fd39f75ab288ab7d1e89)  

## Docker files  
* https://github.com/atoris1192/docker-node-env  
```
git clone https://github.com/atoris1192/docker-node-env.git

git reset --hard c026b7e19331000aa0a

cd docker-node-env
docker-composer up
docker-compose run --service-ports node bash

cd src
yarn
npx parcel index.pug
```

# Front End, Test Tool  
[フロントエンド・テストツール比較 Selenium #01環境構築編 updated at 2018-03-29](https://qiita.com/creaith/items/c138c0417470a17cc4b4)  
[フロントエンド・テストツール比較 Selenium #02テスト編 updated at 2018-03-30](https://qiita.com/creaith/items/e654fae65811a0df74f3)  
[フロントエンド・テストツール比較 Selenium #03初心者でもわかる入門編 updated at 2019-01-12](https://qiita.com/creaith/items/b1e9e04a70e0059ee601)  


# Node.js and iPhone by iBeacon Test
[Node.jsとiPhoneでiBeaconの送受信テスト updated at 2014-02-05](https://qiita.com/hokaccha/items/1592cd03ee649e037b89)  


# Troubleshooting


# Reference

## supertest  
[visionmedia /supertest](https://github.com/visionmedia/supertest)
> Super-agent driven library for testing node.js HTTP servers using a fluent API.     

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

