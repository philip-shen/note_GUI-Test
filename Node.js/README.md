
Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Purpose](#purpose)
   * [Node.js Test Tool](#nodejs-test-tool)
      * [Installation](#installation)
      * [Sample Code](#sample-code)
   * [Node.js and iPhone by iBeacon Test](#nodejs-and-iphone-by-ibeacon-test)
   * [Troubleshooting](#troubleshooting)
   * [Reference](#reference)
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

# Node.js and iPhone by iBeacon Test
[Node.jsとiPhoneでiBeaconの送受信テスト updated at 2014-02-05](https://qiita.com/hokaccha/items/1592cd03ee649e037b89)  


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
