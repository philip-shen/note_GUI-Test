
Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Purpose](#purpose)
   * [TestLink Setup](#testlink-setup)
      * [TestLink Environment Setup](#testlink-environment-setup)
      * [TestLink Installation](#testlink-installation)
      * [Updated docker-compose.yml](#updated-docker-composeyml)
      * [Password of DB  docker-compose.yml](#password-of-db--docker-composeyml)
         * [getting "mkdir: cannot create directory '/bitnami/mariadb': Permission denied" #193](#getting-mkdir-cannot-create-directory-bitnamimariadb-permission-denied-193)
         * [ERROR: Couldn’t connect to Docker daemon at http docker://localunixsocket - is it running?](#error-couldnt-connect-to-docker-daemon-at-httpdockerlocalunixsocket---is-it-running)
         * [Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?](#cannot-connect-to-the-docker-daemon-at-unixvarrundockersock-is-the-docker-daemon-running)
         * [Cannot connect to the Docker daemon at tcp://localhost:2375. Is the docker daemon running?](#cannot-connect-to-the-docker-daemon-at-tcplocalhost2375-is-the-docker-daemon-running)
         * [MariaDB Data Perpetuation on Docker for Windows](#mariadb-data-perpetuation-on-docker-for-windows)
         * [MariaDB Installation on WSL](#mariadb-installation-on-wsl)
      * [TestLink Activatation and Login](#testlink-activatation-and-login)
      * [9. 参考：最終的な custom_config.inc.php の設定例](#9-参考最終的な-custom_configincphp-の設定例)
      * [Requirement](#requirement)
         * [Jenkins Installation](#jenkins-installation)
         * [TestLink Installation](#testlink-installation-1)
         * [Gitea Installation](#gitea-installation)
      * [Deployment](#deployment)
   * [TestLink and USDM(Universal Specification Describing Manner)](#testlink-and-usdmuniversal-specification-describing-manner)
      * [HAYST(Highly Accelerated and Yield Software Testing) and FV Table(Function Verification Table)](#haysthighly-accelerated-and-yield-software-testing-and-fv-tablefunction-verification-table)
      * [USDM and FV Table vs TestLink Data](#usdm-and-fv-table-vs-testlink-data)
   * [TestLink Tutorial](#testlink-tutorial)
      * [Setting up a Project](#setting-up-a-project)
      * [Creating a Test Plan](#creating-a-test-plan)
      * [How to Create a Build](#how-to-create-a-build)
      * [Test Suite Management Documentation](#test-suite-management-documentation)
      * [Creating a Testcase](#creating-a-testcase)
      * [Assigning test case to test plan](#assigning-test-case-to-test-plan)
      * [How to execute Test Cases in TestLink](#how-to-execute-test-cases-in-testlink)
      * [How to Create Users and Assign Roles Tutorial](#how-to-create-users-and-assign-roles-tutorial)
      * [Writing Requirements in TestLink](#writing-requirements-in-testlink)
      * [Assigning the Requirements to Test Case(s)](#assigning-the-requirements-to-test-cases)
   * [Manits Installation](#manits-installation)
   * [Troubleshooting](#troubleshooting)
   * [Reference](#reference)
   * [h1 size](#h1-size)
      * [h2 size](#h2-size)
         * [h3 size](#h3-size)
            * [h4 size](#h4-size)
               * [h5 size](#h5-size)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)


# Purpose  
Take note of TestLink  


# TestLink Setup  

[テスト工程の管理をするツール、TestLinkについて ](https://qiita.com/mima_ita/items/ed56fb1da1e340d397b9)  
## TestLink Environment Setup  
[TestLinkの環境作成](https://qiita.com/mima_ita/items/ed56fb1da1e340d397b9#testlink%E3%81%AE%E7%92%B0%E5%A2%83%E4%BD%9C%E6%88%90)  
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F47856%2Fae024b25-aa46-392e-6925-53436ccb5198.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=5d4dc6732d344a9edd447c3d956a8f16)  

## TestLink Installation  
[【2019年版】macOS 10.14 Mojaveの docker で TestLink をインストールと設定する Nov 19, 2019](https://qiita.com/shimizumasaru/items/a1de689866c3ee2a4de5)  

```
$ mkdir -p ~/testlink/backup/
$ cd ~/testlink
$ curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-testlink/master/docker-compose.yml > docker-compose.yml
```

```
編集ポイント
- image: を bitnami/mariadb:10.3 に指定する
- image: を bitnami/testlink:1.9.19 に指定する
- ALLOW_EMPTY_PASSWORD=yes で DB パスワード無しにする
　※簡易だが公開サーバには不向き、DBパスワードありは後述する
- ローカルPCの~/testlink/のバックアップディレクトリを Volumes: で指定する
- ポート番号を任意のポートに指定する
- メール設定をする（本設定は Gmail のもの）
- TestLink の管理者アカウントとパスワードを指定する
- TestLink の言語を日本語対応させる
```

## Updated docker-compose.yml  
```
version: '3'

services:
  mariadb:
    image: 'bitnami/mariadb:10.3'
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_testlink
      - MARIADB_DATABASE=bitnami_testlink
    volumes:
      - '~/testlink/backup/mariadb:/bitnami'
  testlink:
    image: 'bitnami/testlink:1.9.19'
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_HOST=mariadb
      - MARIADB_PORT_NUMBER=3306
      - TESTLINK_DATABASE_USER=bn_testlink
      - TESTLINK_DATABASE_NAME=bitnami_testlink
      - TESTLINK_EMAIL=mymailaddress@gmail.com
      - TESTLINK_LANGUAGE=ja_JP
      - SMTP_ENABLE=true
      - SMTP_HOST=smtp.gmail.com
      - SMTP_PORT=587
      - SMTP_USER=mymailaddress@gmail.com
      - SMTP_PASSWORD=mymailpassword12345678
      - SMTP_CONNECTION_MODE=tls
      - TESTLINK_USERNAME=admin
      - TESTLINK_PASSWORD=pass1234
    ports:
      - '0.0.0.0:33080:80'
      - '0.0.0.0:33443:443'
    volumes:
      - '~/testlink/backup/testlink:/bitnami'
    depends_on:
      - mariadb
```

## Password of DB  docker-compose.yml  
```
version: '3'

services:
  mariadb:
    image: 'bitnami/mariadb:10.3'
    environment:
      - MARIADB_ROOT_PASSWORD=master_root_password
      - MARIADB_PASSWORD=my_password
      - MARIADB_USER=bn_testlink
      - MARIADB_DATABASE=bitnami_testlink
    volumes:
      - '~/testlink/backup/mariadb:/bitnami'
  testlink:
    image: 'bitnami/testlink:1.9.19'
    environment:
      - TESTLINK_DATABASE_PASSWORD=my_password
      - MARIADB_HOST=mariadb
      - MARIADB_PORT_NUMBER=3306
      - TESTLINK_DATABASE_USER=bn_testlink
      - TESTLINK_DATABASE_NAME=bitnami_testlink
      - TESTLINK_EMAIL=mymailaddress@gmail.com
      - TESTLINK_LANGUAGE=ja_JP
      - SMTP_ENABLE=true
      - SMTP_HOST=smtp.gmail.com
      - SMTP_PORT=587
      - SMTP_USER=mymailaddress@gmail.com
      - SMTP_PASSWORD=mymailpassword12345678
      - SMTP_CONNECTION_MODE=tls
      - TESTLINK_USERNAME=admin
      - TESTLINK_PASSWORD=pass1234
    ports:
      - '0.0.0.0:33080:80'
      - '0.0.0.0:33443:443'
    volumes:
      - '~/testlink/backup/testlink:/bitnami'
    depends_on:
      - mariadb
```

### getting "mkdir: cannot create directory '/bitnami/mariadb': Permission denied" #193  
[Solving Bitnami Docker Redmine ‘cannot create directory ‘/bitnami/mariadb’: Permission denied’ Dec 15, 2018](https://techoverflow.net/2018/12/15/solving-bitnami-docker-redmine-cannot-create-directory-bitnami-mariadb-permission-denied/)  
```
sudo chown -R 1001:1001 <directory>
```

```
# Example: This can be found in the mariadb section:
    volumes:
      - '/var/lib/myredmine/mariadb_data:/bitnami'
# Example: This can be found in the redmine section
    volumes:
      - '/var/lib/myredmine/redmine_data:/bitnami'
```

```
sudo chown -R 1001:1001 /var/lib/myredmine/mariadb_data /var/lib/myredmine/redmine_data
```

```
docker-compose down

docker-compose up # Use 'docker-compose up -d' to run in the background
```

[getting "mkdir: cannot create directory '/bitnami/mariadb': Permission denied" #193 25 Aug 2019](https://github.com/bitnami/bitnami-docker-mariadb/issues/193)
```
Using the first approach (the one documented in the guide), you need to ensure that the local folder (mariadb-data) has the correct permissions, you can do it just assigning write permissions to others:

sudo chmod o+w mariadb-data

After that, you should be able to run

docker-compose up -d
```
```
Using the second approach, you don't need to create the folder or set the proper permissions because the volume is created using the Docker Engine driver as an object instead of a real directory in your filesystem (I think this is the best approach for databases), you only need to edit the docker-compose.yml, adding the following lines to the end

volumes:
  mariadb-data:
    driver: local
```
```
There are 2 options to solve the issue:
Option 1:

    mkdir mariadb-data && sudo chmod o+w mariadb-data
    docker-compose up

Option 2:

    Replace ./mariadb-data:/bitnami by mariadb-data:/bitnami (removing ./) in the docker-compose.yml and add the following lines at the end of the file

volumes:
  mariadb-data:
    driver: local

    docker-compose up
```

### ERROR: Couldn’t connect to Docker daemon at http+docker://localunixsocket - is it running?   
[[ Solution ] 啟動 docker-compose 發生 ERROR: Couldn’t connect to Docker daemon at http+docker://localunixsocket - is it running? 錯誤 Dec, 19 2018](https://oranwind.org/-solution-qi-dong-docker-compose-fa-sheng-error-couldnt-connect-to-docker-daemon-at-httpdockerlocalunixsocket-is-it-running-cuo-wu/)  
> Step 1. 將當前用戶加入 docker 群組  
```
  sudo gpasswd -a ${USER} docker
```
> Step 2. 退出當前用戶  
```
  sudo su
```
> Step 3. 再次切换到 ubuntu 用戶  
```
  su ubuntu
```
> Step 4. 啟動 docker-compose  
```
  docker-compose up -d
```

### Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?  
[Installing the Docker client on Windows Subsystem for Linux (Ubuntu) Oct 21, 2017 ](https://medium.com/@sebagomez/installing-the-docker-client-on-ubuntus-windows-subsystem-for-linux-612b392a44c4)  

> You need to tell the Docker client where the Docker host is, and you can do that by using the -H option as follows:  
```
$ docker -H localhost:2375 images
```

> If you don't want to type the host every time, you can set up and environment variable called DOCKER_HOST to localhost:2375
```
$ export DOCKER_HOST=localhost:2375
```

> But, that environment variable will last only as long as the session does. You would have to set it every time you open bash. So, in order to avoid that, you set that variable in a file called .bash_profile in your home directory, like this:  
```
$ echo "export DOCKER_HOST=localhost:2375" >> ~/.bash_profile
```

[Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running? Dec 23, 2017](https://forums.docker.com/t/cannot-connect-to-the-docker-daemon-at-unix-var-run-docker-sock-is-the-docker-daemon-running/43371)  

> This TCP endpoint is turned off by default; to activate it, right-click the Docker icon in your taskbar and choose Settings, and tick the box next to “Expose daemon on tcp://localhost:2375 without TLS”.   
> With that done, all we need to do is instruct the CLI under Bash to connect to the engine running under Windows instead of to the non-existing engine running under Bash, like this:  
```
$ docker -H tcp://0.0.0.0:2375 images
```

> There are two ways to make this permanent – either add an alias for the above command or export an environment variable which instructs Docker where to find the host engine:  
```
$ echo “export DOCKER_HOST=‘tcp://0.0.0.0:2375’” >> ~/.bashrc
$ source ~/.bashrc
```

[Running Docker containers on Bash on Windows April 19, 2017](https://blog.jayway.com/2017/04/19/running-docker-on-bash-on-windows/)  
> 2. Connect Docker on WSL to Docker on Windows  
```
export DOCKER_HOST='tcp://0.0.0.0:2376'
```

> Of course, we’d want these changes in .bashrc  to make them sticky:  
```
$ echo >> ~/.bashrc <<EOF
# Connect to Docker on Windows
export DOCKER_CERT_PATH=/mnt/c/Users/YOUR_USERNAME/.docker/machine/certs
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST='tcp://0.0.0.0:2375'
EOF
$ source ~/.bashrc
```

> Now, running docker commands from Bash works just like they’re supposed to.  
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

[ubuntu running under WSL2 not seeing Docker daemon at unix:///var/run/docker.sock #5096 8 Nov 2019](https://github.com/docker/for-win/issues/5096)  

### Cannot connect to the Docker daemon at tcp://localhost:2375. Is the docker daemon running?  
![alt tag](https://camo.githubusercontent.com/dc9194e2af1ab56455de1aa4b571615bc3a69442/68747470733a2f2f692e696d6775722e636f6d2f746669634759462e6a7067)  
```
sudo docker images

sudo docker version
```

### MariaDB Data Perpetuation on Docker for Windows   
[Docker for WindowsでMariaDBのデータを永続化 Sep 12, 2019](https://qiita.com/imp555sti/items/3382272b6c1eada4711c)  
> docker-compose.yaml(成功例)  
> MariaDB用のVolumeをコンテナ化しているのがポイントとなります。  
```
docker-compose.yaml

version: '3'
services:
  mariadb:
    container_name: laravel_mariadb
    build: ./mariadb/
    image: laravel_mariadb:10.4.7
    ports:
      - 3306:3306
    volumes:
      - dbvolume/data:/var/lib/mysql          # ← 問題の箇所
    environment:
      - MYSQL_DATABASE=laravel_db
      - MYSQL_ROOT_PASSWORD=root@1234
      - MYSQL_USER=laravel
      - MYSQL_PASSWORD=laravel@1234

volumes:                                      # ← Volumesでデータ領域をコンテナとして定義
  dbvolume:
```

### MariaDB Installation on WSL   
[UbuntuにMariaDBをインストールする Apr, 3 2019](https://utsu-programmer.com/environment/mariadb-install/)  
> MariaDBリポジトリを取得する  
```
$ curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

```
$ apt-get -y update

$ apt-get -y upgrade
```

> MariaDBのインストール  
```
$ apt-cache search mariadb-server
```
```
$ apt-get install mariadb-server-10.3
```
```
$ dpkg -l | grep -i mariadb | grep -v 10.1
```

> mysqld.sock (2)　エラー   
```
$ mysql -u root -p
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
```

```
$ sudo touch /var/run/mysqld/mysqld.sock
```

> mysqld.sock (13)　エラー   
```
$ service mysql start
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (13)
```

```
$ sudo chmod 777 /var/run/mysqld/mysqld.sock
```

> MariaDBの起動 
```
$ service mysql start
```

```
$ mysql -u root -p
```

> 文字コードの設定   
```
$ vi /etc/mysql/my.cnf
```

```
[mysqld]　セクション内
character-set-server=utf8

[mysqldump]　セクション内
default-character-set=utf8

[mysql]　セクション内
default-character-set=utf8
```

```
$ service mysql  restart
```


[Docker Desktop WSL 2 backend ](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)  
[]()  


[【2019年版】Docker Bitnami/TestLink を設定する Nov 19, 2019](https://qiita.com/shimizumasaru/items/6c7b3f55a2dd63d5252b)  

## TestLink Activatation and Login  
![alt tag](https://i.imgur.com/Tf8ucDO.png)  

![alt tag](https://i.imgur.com/n72yr2o.png)  

## 9. 参考：最終的な custom_config.inc.php の設定例  
[9. 参考：最終的な custom_config.inc.php の設定例](https://qiita.com/shimizumasaru/items/6c7b3f55a2dd63d5252b#9-%E5%8F%82%E8%80%83%E6%9C%80%E7%B5%82%E7%9A%84%E3%81%AA-custom_configincphp-%E3%81%AE%E8%A8%AD%E5%AE%9A%E4%BE%8B)

```
<?php

// 参考資料： TestLink インストールマニュアルや、各種設定ドキュメントは、
// /opt/bitnami/testlink/docs 配下に存在する

// path設定
$tlCfg->log_path = '/opt/bitnami/testlink/logs/';
$g_repositoryPath = '/opt/bitnami/testlink/upload_area/';

// TestLinkセキュリティ上の弱点が存在する場合に警告する方法は、
//  * 'SCREEN'：ログイン画面にメッセージを表示する（デフォルト）
//  * 'FILE'：警告ファイルを作成、ログイン画面にメッセージを表示する。
//  * 'SILENT'：警告ファイルを作成、ログイン画面にメッセージを表示しない。
$tlCfg->config_check_warning_mode = 'SCREEN';

// smtp設定
$g_smtp_host = 'smtp.gmail.com';
$g_tl_admin_email = 'mymailaddress@gmail.com';
$g_from_email = 'mymailaddress@gmail.com';
$g_return_path_email = 'mymailaddress@gmail.com';
$g_smtp_username = 'mymailaddress@gmail.com';
$g_smtp_password = 'mymailpassword12345678';
$g_smtp_port = '587';
$g_smtp_connection_mode = 'tls';

// デフォルト言語を日本語にする
$tlCfg->default_language = 'ja_JP';

// レポートで表示される TestLink ロゴを自社ロゴに変更する
$tlCfg->document_generator->company_logo = 'logo.png';

// ロゴマークの高さを指定する
$tlCfg->document_generator->company_logo_height = '50';

// グラフのレポートのシステムフォントを IPA P ゴシック(Ver.003.03)に設定する
$tlCfg->charts_font_path = "/usr/share/fonts/truetype/ipagp00303/ipagp.ttf";

// TestLink にアップロードするファイルサイズ制限を拡大する。ただし(多くの場合のPHPデフォルト値の)２MBを超える場合は、同時に php.ini の upload_max_filesize の設定も変更すること
$tlCfg->import_max_size = '100000000'; // in bytes
$tlCfg->repository_max_filesize = 100; //MB

// レポートとメトリクスで表示される会社名を設定する
$tlCfg->document_generator->company_name = '自社名';
$tlCfg->document_generator->company_copyright = 'Copyright© ' . date('Y') . '自社名';
$tlCfg->document_generator->confidential_msg = '本資料は、自社名の許可無く対外的に参照・配布しないようお願い申し上げます。';

// 実行済みのテストケースの編集を可能にする
$tlCfg->testcase_cfg->can_edit_executed = ENABLED;

// 実行済みのテストケースの削除不可にする(初期値：削除可能)
$tlCfg->testcase_cfg->can_remove_executed = DISABLED;

// ユーザをテストケースにアサインしなくても、テスト実行およびレポートを出力する
$tlCfg->exec_cfg->exec_mode->tester='all';

// テスト実行時にアサインに関わらず、テストケースを全て表示する
$tlCfg->exec_cfg->user_filter_default='none';

// テストの実行で、実行履歴を表示する
$tlCfg->exec_cfg->history_on = TRUE;

// sessionInactivityTimeoutを延長する
$tlCfg->sessionInactivityTimeout = 86400;

// 管理者による新規アカウント登録に限定する
$tlCfg->user_self_signup = FALSE;

?>
```

[TestLinkで手動テストと自動テストを統合する : Jenkins編 an 30, 2016](https://qiita.com/ootaken/items/a090890ea08f8dbd5b61)  
[Docker ComposeでJenkinsとSelenium Gridを一気に立ち上げる updated at 2016-07-22](https://qiita.com/ootaken/items/26d37cb34d3b575277e0)  

[Jenkins Testlink Plug-in使用总结 2016-11-10](https://blog.csdn.net/TalorSwfit20111208/article/details/53112885)  

[自動化測試(檢查), 整合TestLink x Jenkins x Gitea x pytest May 21, 2018](https://medium.com/@canyu/%E8%87%AA%E5%8B%95%E5%8C%96%E6%B8%AC%E8%A9%A6-%E6%AA%A2%E6%9F%A5-%E6%95%B4%E5%90%88-testlink-x-jenkins-x-gitea-x-pytest-c5f1c4a71cfb)  
```
用 Jenkins 執行以 pytest 撰寫自動化測試(檢查)並回填結果到 TestLink
既然要整合這些服務，那當然要先建起來。
要感謝 Docker 的出現讓生命快樂了許多….開心地使用 docker-compose up 吧!
```
## Requirement  
### Jenkins Installation  
```
version: '2'
services:
  jenkins:
    images: 'jenkins:lts'
    volumes:
      - 'jenkins_home:/var/jenkins_home'
    ports:
      - '8080:8080'
      - '50000:50000'
volumes:
  jenkins_data:
    driver: local
```

### TestLink Installation  
```
version: '2'
services:
  mariadb:
    image: 'bitnami/mariadb:latest'
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_testlink
      - MARIADB_DATABASE=bitnami_testlink
    volumes:
      - 'mariadb_data:/bitnami'
  testlink:
    image: 'bitnami/testlink:latest'
    ports:
      - '80:80'
      - '443:443'
    volumes:
      - 'testlink_data:/bitnami'
    depends_on:
      - mariadb
    environment:
      - MARIADB_HOST=mariadb
      - MARIADB_PORT_NUMBER=3306
      - TESTLINK_DATABASE_USER=bn_testlink
      - TESTLINK_DATABASE_NAME=bitnami_testlink
      - ALLOW_EMPTY_PASSWORD=yes
      - TESTLINK_USERNAME=admin
      - TESTLINK_PASSWORD=password
      - TESTLINK_EMAIL=admin@example.com
volumes:
  mariadb_data:
    driver: local
  testlink_data:
    driver: local
```

### Gitea Installation  
```
version: '2'
services:
  gitea:
    image: 'gitea/gitea:latest'
    volumes:
      - gitea_data:/data/
    ports:
      - '10022:10022'
      - '3000:3000'
volumes:
  gitea_data:
    driver: local
```


## Deployment    
```
Testlink 建立Project, Test Plan, Testcase, 打開 Automation, 產生個人 API Key

Jenkins 安裝 Testlink Plugin

Jenkins 設定 Testlink 服務位置跟 API Key

寫 python 測試上傳到 gitea

Jenkins 建立 Job 設定 git source, testlink設定 (Custom Fields 一定要填上), 收xml

執行 Job 看看有沒錯誤, 最後上 Testlink 看看 TestPlan 的 build 的結果是不是正確的填回去了
```

# TestLink and USDM(Universal Specification Describing Manner)  
[要求管理とテスト管理、その間のトレーサビリティの維持、および要求(仕様)カバレッジの把握 －－－ TestLinkとExcelツールで運用する Jul 25, 2019](https://qiita.com/sho1884/items/d1ce380f6872ba2454fa) 

## HAYST(Highly Accelerated and Yield Software Testing) and FV Table(Function Verification Table)  
> ここではその部分にテスト設計技法の一つとして知られるHAYST(Highly Accelerated and Yield Software Testing)法のFV表(Function Verification Table) を使うことにします。  

![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F246520%2Faa91c33d-8422-d4f2-7bef-ec08d0c94ca8.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=ed5c5fe7f41e9a254b9883cb2e086c79)  

## USDM and FV Table vs TestLink Data
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F246520%2Fb84bdd4c-9156-85ca-05e0-66c18449528f.png?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&w=1400&fit=max&s=6e810c8a8407b98bc2278d370c4e3294)  


# TestLink Tutorial  
[TestLink Tutorial – Getting Started Documentation Last updated on: Jan 17, 2020](https://qa.world/testlink-tutorial/)  

## Setting up a Project  

![alt tag](https://qa.world/wp-content/uploads/2019/04/testlink-project.png)  

## Creating a Test Plan  
* Step 1: Navigate to the Desktop page by clicking on the icon in the top bar as shown in the screenshot below.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-plan-1200x324.png)  

* Step 2: Once a new page is loaded, you can see a Test Plan Management option on the right side of the page. A new page will appear with a button ‘Create’, you have to click that.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-plan-2-1200x343.png)  

* Step 3: Provide information in the fields and click on the Create button at the bottom to finish creating a test plan.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-plan-step-3.png)  

* You can edit and delete a test plan.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-plan-step-4-1200x250.png)  

## How to Create a Build  
* Step 1: Click on Builds/Releases option in the right side of the dashboard as shown in the screenshot below.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/testlink-builds-1200x345.png)  

* A new screen will appear, asking you to click on the button ‘Create’.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/no-builds.png)  

* Step 2: Provide the necessary details and click on the ‘Create’ button at the bottom to successfully create a build.  
```
The build should have a unique title.

There are two attributes: active and inactive. Active is set by default, 
and defines whether the build can be used or not. When we select inactive build, 
then these will not be listed anywhere on reports or execution pages.
    
A release date is a future date when the build will be released.
    
The build can be edited and deleted as well.
```
![alt tag](https://qa.world/wp-content/uploads/2019/04/testlink-new-build.png)  

## Test Suite Management Documentation  
* Step 1: Click on the ‘Test Specification’ option in the bottom left of the screen on the dashboard. Upon clicking the option, the following screen will appear.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-suite-1-1200x336.png)  

* Step 2: Fill all the necessary information and click on ‘Save’ button.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/create-test-suite-1200x607.png)  

## Creating a Testcase  
[TestLink Tutorial: A Complete Guide](#https://www.guru99.com/testlink-tutorial-complete-guide.html#18)  
[Creating a Testcase](https://www.guru99.com/testlink-tutorial-complete-guide.html#18)  

* Step 1: Click on the test suite folder on the left side of the panel under a folder tree structure  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut18.png)  


* Step 2: Click on the setting icon in the right side panel. List of test case operations will be displayed on the right side panel  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut19.png)  


* Step 3: New window will open, to create test cases click on create a button in test-case operations  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut20.png)  


* Step 4: Enter the details in the test case specification page  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut21.png)  


* Step 5: After entering the details, click on "create" button to save the details. The test-case for Guru99 is created successfully  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut22.png)  


* Step 6: Click on test-case from the folder, as shown above, it will open a window. Click on "create steps" button in a test case. It will open a test case step editor  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut23.png)  


* Step 7: It will open another window on the same page, in that window you have to enter the following details

  * 1. Enter the step-action for your test case
  * 2. Enter the details about the step action
  * 3. Click save it and add another step action OR click save and exit tab if there is no more test step to add

![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut24.png)  


* Step 8: Once you save and exit the test step, it will appear like this 
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut25.png)  

## Assigning test case to test plan  
[Assigning test case to test plan](https://www.guru99.com/testlink-tutorial-complete-guide.html#19)  

* Step 1: Click on the setting icon TestLink Tutorial: A Complete Guide on the test panel. It will show the list of operations. 

* Step 2: Click on "Add to Test Plans"  
![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut27.png)  


* Step 3: New window will open, search your project "Guru99"  

  * 1.  Mark the checkbox against your test plan
  * 2.  Click on add button

![alt tag](https://www.guru99.com/images/1-2015/011915_1034_TestLinkTut28.png)  


## How to execute Test Cases in TestLink  
* Step 1: As with step 1 of the test suite, navigate to Test Specification and click on the option as shown below (TS-001-ABC).  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-case-1.png)  

* Step 2: Click on the settings icon which is marked in black and it will open two options beneath it. Click on the (+) icon to the right of the words ‘Test Case Operations’.

* Step 3: Fill out all the necessary information and click on ‘Create’ button in the form.
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-case-step-3.png)  

* Test cases can be edited and deleted as required, and testers can easily create further steps for more efficiency by clicking the ‘Create Step’ button.

![alt tag](https://qa.world/wp-content/uploads/2019/04/test-case-structure-1200x553.png)  

* You can then provide necessary information and click on ‘Create’ button at the bottom as shown in the below screenshot. It will link the steps with the respective test case.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/add-test-case-step.png)  

## How to Create Users and Assign Roles Tutorial  

* Guest: A guest cannot edit, but only view test cases and metrics.  

* Tester: A tester only has access to execute test cases and view test metrics.

*  Senior Tester: A senior tester can view, create, modify, and delete test cases as needed. Similar to a regular tester role, a senior tester can execute test cases and can view test metrics. The senior tester role cannot create test cases in TestLink.

*  Test Lead (Leader) & Admin: These roles can perform all major functionalities such as creating a test plan and build, and are able to alter them at any stage. They can also execute test cases, and assign roles to users along with permissions.  


* Step 1:  Click on the User icon in the navigation bar as shown below. A new page will open.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/assign_role.png)  

* Step 2: Click the ‘Create’ button present on that page.

* Step 3: Enter all required details on the form.
![alt tag](https://qa.world/wp-content/uploads/2019/04/user-details.png)  

* Step 4: As a new user is created, you can now assign a project to that particular user. To do so, click on the tab ‘Assign Test Project roles’.

* Step 5: Select a project name and assign a role to that user, then click the ‘Update’ button present on that page, as shown in the picture below.
![alt tag](https://qa.world/wp-content/uploads/2019/04/select-testlink-project-1200x441.png)  

## Writing Requirements in TestLink  
> A requirement details qualities that a function should possess in order to perform as envisioned.   

* Step 1: Click on the ‘Requirements’ icon in the navigational tab as shown in the picture.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/writing_req.png)  

* Step 2:  A new page will open. Click on ‘Create’ button.

* Step 3: Fill out the form with the necessary information and click on ‘Save’ button.
![alt tag](https://qa.world/wp-content/uploads/2019/04/testlink-requirement.png)  

## Assigning the Requirements to Test Case(s)  
> One of the important activities for the test team is to track test coverage so that no requirements can be missed. In TestLink, one can link the requirements to the test cases by following few of the below steps.  

* Step 1: Navigate to ‘Test Specification’ section and open an existing test case.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/test-spec.png)  

* Step 2: As you can see in the above screenshot, a ‘Requirement’ button is present along with an icon. You have to click that file icon button in order to link the test cases with the requirements. A dialogue box will then appear.  
![alt tag](https://qa.world/wp-content/uploads/2019/04/testlink-spec-document.png)  

* Step 3: After selecting your chosen requirements, click the ‘Assign’ button and the operation will be executed successfully.  


[TestLink Tutorial: A Layman’s Guide to TestLink Test Management Tool (Tutorial #1) April 16, 2020](https://www.softwaretestinghelp.com/testlink-tutorial-1/)  
[How to Manage Requirements, Execute Test Cases and Generate Reports Using TestLink – Tutorial #2 April 16, 202](https://www.softwaretestinghelp.com/testlink-tutorial-2/)  
[How to Update TestLink Test Case Execution Status Remotely Through Selenium – Tutorial #3 April 16, 202](https://www.softwaretestinghelp.com/testlink-tutorial-3/)  
[TestLink Tutorial 4 – Test Metrics, Keyword Management, Custom Fields and Test Report Charts April 16, 202](https://www.softwaretestinghelp.com/testlink-tutorial-4/)  

# Manits Installation  

[PortainerでMantis Bug Tracker2.15とMariaDBのスタックを作成する Jun 15, 2018](https://serverarekore.blogspot.com/2018/07/portainermantis-bug-tracker215mariadb.html)  

> Dockerfile   

```
FROM alpine:3.7
WORKDIR /
RUN  apk update \
  && apk add --no-cache apache2 php7-apache2 php7-mysqli php7-pdo php7-pdo_mysql php7-intl php7-mcrypt php7-mbstring php7-session php7-curl php7-json php7-xml php7-zip php7-tokenizer php7-ctype php7-gd php7-xmlwriter php7-xmlreader php7-dom php7-iconv php7-simplexml php7-fileinfo openssl openrc unzip \
  && rm -rf /var/cache/apk/* \
  && wget https://sourceforge.net/projects/mantisbt/files/mantis-stable/2.15.0/mantisbt-2.15.0.tar.gz \
  && tar xvfz mantisbt-2.15.0.tar.gz \
  && rm -f mantisbt-2.15.0.tar.gz \
  && mkdir -p /opt \
  && mv mantisbt-2.15.0 /opt/mantisbt \
  && chown -R apache:apache /opt/mantisbt \
  && ln -s  /opt/mantisbt /var/www/localhost/htdocs \
  && mkdir -p /run/apache2 \
  && mkdir -p /var/mantisbt \
  && chown -R apache:apache /var/mantisbt
EXPOSE 80
VOLUME ["/var/mantisbt","/opt/mantisbt"]
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

> docker-compose.yml  

```
version: "3"
services:
  mantisbt:
    image: mantisbt:2.15
    container_name: "mantisbt"
    volumes:
      - "mantisbt-data:/var/mantisbt"
      - "mantisbt-opt:/opt/mantisbt"
    ports:
      - "13980:80"
    depends_on:
      - mantisbtdb
  mantisbtdb:
    image: mariadb:10.3
    command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    container_name: "mantisbt-db"
    ports:
      - "13906:3306"
    volumes:
      - "mantisbtdbdb-data:/var/lib/mysql"
    environment:
        MYSQL_DATABASE: mantisbt
        MYSQL_ROOT_PASSWORD: mantisbt
volumes:
  mantisbtdbdb-data:
    driver: local
  mantisbt-data:
    driver: local
  mantisbt-opt:
    driver: local
```

> ブラウザからhttp://<Dockerホスト名>:13980/mantisbt/にアクセスする 
```
Installer Optionsで以下の項目を入力する
Type of Database: MySQL Improved
Hostname (for Database Server): mantisbtdb
Username (for Database): root
Password (for Database): mantisbt
Database name (for Database): mantisbt
```

![alt tag](https://1.bp.blogspot.com/-GmXq2XGe8pU/WzelFHzxYjI/AAAAAAAAOXA/1tFAytA3OdUsmZ1K_z-ONwbu1z5rAU4pgCLcBGAs/s640/portainer-mantisbt-p4.png@448w_518h.jpg)  

[xlrl/mantisbt](https://hub.docker.com/r/xlrl/mantisbt) 
> The examples suppose you will have the data for your containers in /srv/mantis. Adapt for your server.
> docker-compose.yml 

```
mantisbt:
  image: xlrl/mantisbt:latest
  ports:
    - "33980:80"
  links:
    - mysql
  volumes:
    - ./config:/var/www/html/config
    - ./custom:/var/www/html/custom
  restart: always

mysql:
  image: mariadb:latest
  environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_DATABASE=bugtracker
    - MYSQL_USER=mantisbt
    - MYSQL_PASSWORD=mantisbt
  volumes:
    - ./mysql:/var/lib/mysql
```


[Setting up PHP-FPM, Nginx, Mariadb on CentOs using docker ](https://stackoverflow.com/questions/52540785/setting-up-php-fpm-nginx-mariadb-on-centos-using-docker/52628260)  

* [https://github.com/matchish/skeleton](https://github.com/matchish/skeleton)
```
You can run terminal in any service

docker-compose exec db bash
docker-compose exec php-fpm bash
docker-compose exec nginx bash 
```

[novatechweb /docker-mantisbt](https://github.com/novatechweb/docker-mantisbt)  

[【玩转开源】基于Docker搭建Bug管理系统 MantisBT Apr 19, 2019](https://www.shuzhiduo.com/A/l1dyVAG0ze/)  
[Setup MantisBT on Ubuntu 18.04 | 16.04 with Nginx Nov 23, 2019](https://websiteforstudents.com/setup-mantisbt-on-ubuntu-18-04-16-04-with-nginx/)  

[技术干货丨如何在Docker环境下搭建测试管理平台？ Apr 2, 2019](https://zhuanlan.zhihu.com/p/61208686)  
[TestLink 與 Mantis 的整合 08/28 2006](http://crystaliris.bokee.com/5588155.html)  


# Troubleshooting


# Reference
[TestLink で LDAP 認証を行う|へっぽこプログラマーの備忘録 2017/10/24](http://kuttsun.blogspot.com/2017/10/testlink-ldap.html)  
[RedmineとTestlinkの連携 03/25 2011](http://in.shappi.org/article/192485671.html)  
[TestLinkの使い方メモ　テストプロジェクトとテスト項目の作成 2010/06/19(土)](https://symfoware.blog.fc2.com/blog-entry-443.html)  
[脱Excel！ Redmineでアジャイル開発を楽々管理 (1/5) 2009/04/14](https://www.atmarkit.co.jp/ait/articles/0904/14/news117.html)  
[脱Excel！ TestLinkでアジャイルにテストをする (2/6) 2009/10/23](https://www.atmarkit.co.jp/ait/articles/0910/23/news110_2.html)  

[オープンソースのテスト管理ツール TestLinkをさくらのレンタルサーバーに設置する 2011/12/2](https://nantekottai.com/2011/12/02/testlink-the-open-source-test-managament-tool/)  
[TestLink1.9.10へのバージョンアップ時の調査 Jul 10, 2014](https://qiita.com/mima_ita/items/f2837a91c9492a2b9e72)  

[基于python+Testlink+Jenkins实现的接口自动化测试框架V3.0 年03/16, 2017](https://testerhome.com/topics/7992)  
[Jackden's Blog 12/9, 2019](https://jackden-diary.blogspot.com/)  
[TestLink 安裝說明 - Krilo 的筆記本 10/25 2006](http://kriloc.blogspot.com/2006/10/)  


[testlink測試用例系統搭建完善文檔，一個不錯的測試管理工具 2018年2月5日](https://kknews.cc/zh-tw/code/nkz2l9q.html) 


* []()  
![alt tag]()  
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
