Table of Contents
=================

   * [Table of Contents](#table-of-contents)
   * [Purpose](#purpose)
   * [Plug-In Installation of Jenkins](#plug-in-installation-of-jenkins)
      * [01 dockerfile](#01-dockerfile)
      * [02 Build docker image](#02-build-docker-image)
      * [03 Run Jenkis Container](#03-run-jenkis-container)
         * [Remove Containers  if something wrong](#remove-containers--if-something-wrong)
      * [04 Eneter Jekins Container to make sure Administrator password](#04-eneter-jekins-container-to-make-sure-administrator-password)
         * [File directory on Windows](#file-directory-on-windows)
         * ["NETSTAT.EXE -a | findstr "8080"" on Windows](#netstatexe--a--findstr-8080-on-windows)
      * [05 Administrator Login](#05-administrator-login)
      * [06 Click "Select Plugins"](#06-click-select-plugins)
      * [07 Select Plugins to plugin and then install](#07-select-plugins-to-plugin-and-then-install)
      * [08 Plugins download](#08-plugins-download)
      * [09 Save and Finsh](#09-save-and-finsh)
      * [10 Start using Jenkins](#10-start-using-jenkins)
      * [11 Try it and Enjoy](#11-try-it-and-enjoy)
      * [12 Creat a Project](#12-creat-a-project)
      * [13 Stop Jenkis Container](#13-stop-jenkis-container)
   * [docker-jenkins-django-tutorial](#docker-jenkins-django-tutorial)
      * [Method 2: Named volume](#method-2-named-volume)
      * [Jenkins First time Login](#jenkins-first-time-login)
   * [Jenkins   Portainer.io](#jenkins--portainerio)
      * [dockerfile](#dockerfile)
      * [Unlock Jenkins](#unlock-jenkins)
      * [Post Installation](#post-installation)
         * [Security](#security)
         * [Plugins](#plugins)
      * [Jenkins Fox One](#jenkins-fox-one)
         * [安裝編譯系統所需要的套件: OpenWRT](#安裝編譯系統所需要的套件-openwrt)
         * [New Project: FreeStyle](#new-project-freestyle)
      * [Jenkins Fox Two](#jenkins-fox-two)
         * [Round 1](#round-1)
   * [Jenkins of Docker version Uprage Tips](#jenkins-of-docker-version-uprage-tips)
      * [前提](#前提)
      * [現象](#現象)
      * [1次調査](#1次調査)
      * [原因の推測](#原因の推測)
      * [2次調査](#2次調査)
   * [Redmine Jenkins GitLab Elasticsearch by Docker](#redminejenkinsgitlabelasticsearch-by-docker)
      * [.env](#env)
      * [docker-compose.yml](#docker-composeyml)
   * [Troubleshooting](#troubleshooting)
   * [Reference](#reference)
      * [Android App Automation Test](#android-app-automation-test)
      * [Jenkins for Android](#jenkins-for-android)
      * [GitLab, Redmine and Jenkins by Docker-Compose](#gitlab-redmine-and-jenkins-by-docker-compose)
         * [nginx.conf](#nginxconf)
      * [Docker上に開発環境一式を構築する](#docker上に開発環境一式を構築する)
      * [Jenkinsを手探りで社内ローカルに立てて詰んだ話](#jenkinsを手探りで社内ローカルに立てて詰んだ話)
      * [JenkinsとSeleniumを使ってWebコンテンツの自動UIテスト環境を作ろう！](#jenkinsとseleniumを使ってwebコンテンツの自動uiテスト環境を作ろう)
      * [JenkinsでCI環境構築チュートリアル ～GitHubとの連携～](#jenkinsでci環境構築チュートリアル-githubとの連携)
      * [JenkinsでCI環境構築チュートリアル ～GitHubからWebサーバーへのデプロイ～](#jenkinsでci環境構築チュートリアル-githubからwebサーバーへのデプロイ)
      * [テスト自動化環境構築](#テスト自動化環境構築)
      * [dockerでjenkins構築（plugin install errorを出さない）](#dockerでjenkins構築plugin-install-errorを出さない)
      * [Jenkinsインストール(Ansible)](#jenkinsインストールansible)
      * [【Jenkins備忘録】Python自動テスト環境構築](#jenkins備忘録python自動テスト環境構築)
      * [Dockerで動くJenkinsから他のコンテナを操作する（Docker outside of Docker）](#dockerで動くjenkinsから他のコンテナを操作するdocker-outside-of-docker)
      * [Dockerでjenkins( SSL)](#dockerでjenkinsssl)
      * [Jenkins CheatSheet — Know The Top Best Practices of Jenkins](#jenkins-cheatsheet--know-the-top-best-practices-of-jenkins)
      * [Devops实践中的CICD工具](#devops实践中的cicd工具)
   * [h1 size](#h1-size)
      * [h2 size](#h2-size)
         * [h3 size](#h3-size)
            * [h4 size](#h4-size)
               * [h5 size](#h5-size)
   * [Table of Contents](#table-of-contents-1)

Created by [gh-md-toc](https://github.com/ekalinin/github-markdown-toc)


# Purpose
Take notes of Jenkis  


# Plug-In Installation of Jenkins   
[プラグインインストール済のJenkins 構築 Docker編 その1 Feb 27, 2019](https://qiita.com/atmaru/items/3707b6f3fca7f4671498)   
* モチベーション  
```
パッとローカルのwin10 PCによく使うPlugin入りのJenkinsを構築したい。
みんなで使うJenkinsさんはあるんだけど、ちょっと周りに迷惑かけずに試したいことがある時に、その時だけ使ってあとは捨てる前提。
手段は大きく分けると2つ。

1.Docker Image作っておいて、必要な時だけコンテナ起動
2.Ansibleなどの構成管理ツールで、自動で構築

机上で考えても両者の優越つけられなかったので、両方やってみましょう。
今回は1.のDocker編です。
```
## 01 dockerfile  
```
FROM jenkins:2.19.4
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

#COPY plugins.txt /usr/share/jenkins/ref/
#RUN /usr/local/bin/install-plugins.sh $(cat /usr/share/jenkins/ref/plugins.txt)
```

## 02 Build docker image  
```
λ docker build -t myjenkins:1 .
λ docker images
```
![alt tag](https://i.imgur.com/F6vP46M.jpg)  

## 03 Run Jenkis Container  
```
d:\project\Jenkis
λ cd home\
```

```
d:\project\Jenkis\home
λ docker run -it -d -v %cd%:/var/jenkins_home/ --name myjenkins-1 -p 8080:8080 myjenkins:1
2812257032b451156a2fe2960459b6a901d814b40fd2b5ecd85655ee80d7a40d

λ docker ps
```
![alt tag](https://i.imgur.com/BHi8eOB.jpg)  

### Remove Containers  if something wrong   
```
d:\project\Jenkis
λ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
490c90a45d61        myjenkins:1         "/bin/tini -- /usr/l…"   24 minutes ago      Exited (1) 24 minutes ago                       myjenkins-1
```
```
d:\project\Jenkis
λ docker rm myjenkins-1
myjenkins-1

d:\project\Jenkis
λ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

## 04 Eneter Jekins Container to make sure Administrator password  
```
λ docker exec -i -t myjenkins-1 bash
jenkins@2812257032b4:/$ cat /var/jenkins_home/secrets/initialAdminPassword
84ffb92f5d694099bef17de4fdb3e687
```
![alt tag](https://i.imgur.com/ItJVJnZ.jpg)  

### File directory on Windows 
![alt tag](https://i.imgur.com/vy3wivw.jpg)  
### "NETSTAT.EXE -a | findstr "8080"" on Windows 
```
d:\project\Jenkis\home
λ NETSTAT.EXE -a | findstr "8080"
  TCP    0.0.0.0:8080           DESKTOP-7EDV2HB:0      LISTENING
  TCP    [::1]:8080             DESKTOP-7EDV2HB:0      LISTENING
```

## 05 Administrator Login  
![alt tag](https://i.imgur.com/iMQ1RfK.jpg)  

## 06 Click "Select Plugins"
![alt tag](https://i.imgur.com/4CRSwFO.jpg)  

## 07 Select Plugins to plugin and then install  
![alt tag](https://i.imgur.com/nD3rAbQ.jpg)  

## 08 Plugins download  
![alt tag](https://i.imgur.com/gE7pfgn.jpg)  

## 09 Save and Finsh  
![alt tag](https://i.imgur.com/FMaRq3i.jpg)  

## 10 Start using Jenkins  
![alt tag](https://i.imgur.com/JqnhRBR.jpg)  

## 11 Try it and Enjoy  
![alt tag](https://i.imgur.com/OnVKD00.jpg) 

## 12 Creat a Project    
![alt tag](https://i.imgur.com/dzSNRzf.jpg)  

## 13 Stop Jenkis Container  
```
d:\project\Jenkis\home
λ docker stop myjenkins-1
myjenkins-1
```
![alt tag](https://i.imgur.com/GKI6YKU.jpg)  



[プラグインインストール済のJenkins 構築 Docker編 番外編 Feb 28, 2019](https://qiita.com/atmaru/items/eba33a407d12cb40f079)   
* 環境や前提条件  
```
windows10 pro 1803
PowerShell
Docker 18.09.0
jenkins:2.19.4
```

[プラグインインストール済のJenkins 構築 Docker編 その2  Mar 01, 2019](https://qiita.com/atmaru/items/1f57b79e4f32c9071b75)  
```
プラグインインストール済のJenkins 構築 Docker編 その1の続きです。
```

[プラグインインストール済のJenkins 構築 Docker編 その4 Mar 03, 2019](https://qiita.com/atmaru/items/25cbc07576e668350d8d)  
```
もともとローカルで動かすのが目的だったが、せっかくなのでAWS上で動かす練習しておく。
（表題と内容が合わないので、あとで表題は変える予定です。）
```


# docker-jenkins-django-tutorial  
[twtrubiks/docker-jenkins-django-tutorial](https://github.com/twtrubiks/docker-jenkins-django-tutorial)  

## Method 2: Named volume    
```
version: '3'
services:

    db:
      image: postgres
      environment:
        POSTGRES_PASSWORD: password123
      ports:
        - "5432:5432"
      volumes:
        - pgdata_jenkins:/var/lib/postgresql/data/

    api:
      build: ./api
      restart: always
      command: python /var/jenkins_home/workspace/demo/api/manage.py runserver 0.0.0.0:8000
      ports:
        - "8002:8000"
      volumes:
        - api_data:/docker_api
        - jenkins_data:/var/jenkins_home
      depends_on:
        - db

    jenkins:
          build: ./jenkins
          restart: always
          ports:
              - "8080:8080"
              - "50000:50000"
          volumes:
              - jenkins_data:/var/jenkins_home
volumes:
    api_data:
    jenkins_data:
    pgdata_jenkins:
```

## Jenkins First time Login  
![alt tag](https://i.imgur.com/kk3UWJm.jpg) 

> cat /var/jenkins_home/secrets/initialAdminPassword
![alt tag](https://i.imgur.com/y63Je7b.jpg) 

![alt tag](https://i.imgur.com/gYAh9dv.jpg) 

![alt tag](https://i.imgur.com/tlaj7sa.jpg) 

![alt tag](https://i.imgur.com/zhcbtfT.jpg) 

![alt tag](https://i.imgur.com/Fke32s6.jpg) 

![alt tag](https://i.imgur.com/9ZfL8zZ.jpg) 

![alt tag](https://i.imgur.com/i0VlVHm.jpg) 


# Jenkins + Portainer.io  
[Jenkins at service  2019-09-26](https://ithelp.ithome.com.tw/articles/10215423)  

## dockerfile  
```
docker run --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

## Unlock Jenkins  
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190920/20094403oXrX4EC1EJ.png)  

![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190920/20094403pD8g13lhlH.png)  

![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190920/20094403AU9i0htAWw.png)  

![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190920/20094403nuHdMWG1ny.png)  

![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190920/20094403tdiyD7GjqV.png)  

## Post Installation  
> restart jenkins by portainer  

### Security  
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190924/20094403uBzrbKf0Uo.png)  

### Plugins  
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190924/20094403c79rutyBRS.png)  

![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190924/20094403gkQ2wNUfyn.png)  

## Jenkins Fox One 

### 安裝編譯系統所需要的套件: OpenWRT  
```
apt-get update && apt-get install -y sudo time git-core subversion build-essential gcc-multilib quilt rsync file libncurses5-dev zlib1g-dev gawk flex gettext wget unzip python vim
```

### New Project: FreeStyle 
>    
![alt tag]()  

> 改變不同參數   
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/20094403x3P1TzIlUp.png)  

> 取得最新原始碼     
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/20094403FvWnGVsxkY.png)  

> 定期建置     
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/20094403qFfQLpdu8j.png)

> make     
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/20094403xiZkBuxsLc.png)  

>    
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/20094403tSWYWCuaIS.png)  

> Console    
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/20094403jq77U17tHl.png)  

> Complete
![alt tag](https://d1dwq032kyr03c.cloudfront.net/upload/images/20190926/200944033pwURvFbcN.png)  

## Jenkins Fox Two  
[Jenkins Fox Two 2019-09-28](https://ithelp.ithome.com.tw/articles/10217573)  

### Round 1  
```
env
```


# Jenkins of Docker version Uprage Tips  
[Docker版JenkinsでのアップグレードTips updated at 2018-12-03](https://qiita.com/nykym/items/6353fcbba679428e0d6e)  

## 前提  
*  Docker 17.05.0-ce  
*  docker-compose 1.8.0  
*  jenkins:2.60.3-alpine  
*  自分のDocker環境で（当初）永続化していたディレクトリーは以下  
>  /var/jenkins_home/     
*  DockerHub上のガイドではアップグレードに関する記載は以下となっている   
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F270508%2F1bf39372-b8c3-60ea-b3ab-cbc4981c2dd8.jpeg?ixlib=rb-1.2.2&auto=format&gif-q=60&q=75&s=52a52ecf98a87662a60a1ea5145920d3)  

## 現象  
*  ブラウザーからUI上でJenkinsのアップデートをかけバージョンアップした  
*  普段のバックアップはdocker stopコマンドで停止した状態で実行しており、アップグレード後にdocker-compose downは未実行  
*  ある日、別件のメンテナンスでdocker-compose downして作業を行いdocker-compose up --build -dを実行したところjenkinsにログインできない状況となった  
   *  エラー「XmlPullParserException」が発生  

## 1次調査  
*  エラー・ログの出力は以下  
> org.xmlpull.v1.XmlPullParserException: only 1.0 is supported as <?xml version not '1.1' (position: START_DOCUMENT seen <?xml version=\'1.1\'... @1:1

## 原因の推測  
*  新しいバージョンのjenkins.warファイルがダウンロードおよび展開されているディレクトリーが永続化されてない可能性が高い

## 2次調査  
*  docker-compose down  
*  xmlファイルを全て1.0に戻す（grepとsedで一括置換）  
```
$ sudo grep -lr srv/jenkins/var/jenkins_home/ --include="*.xml" -e "xml version='1.1'" |
sudo xargs sed -i 's/1.1/1.0/'
```

*  docker-compose up --build -d  
*  コンテナに「docker exec -it /bin/bash」でログインしwarのディレクトリーを調査  
    *  「/usr/share/jenkins/jenkins.war」（warファイルの配置先と推測）  
    *  「/var/cache/jenkins/war」（warの展開先と推測）  
```
$ docker exec -it jenkins_1 /bin/bash
bash-4.3# find / -name *war* -type d
/var/cache/jenkins/war
/var/cache/jenkins/war/META-INF/maven/org.jenkins-ci.main/jenkins-war
/tmp/jetty-0.0.0.0-8080-war-_jenkins-any-781527918347635117.dir
/tmp/jetty-0.0.0.0-8080-war-_jenkins-any-2252951103130751390.dir
/tmp/jetty-0.0.0.0-8080-war-_jenkins-any-5296010582638823514.dir
/lib/firmware
/sys/bus/acpi/drivers/hardware_error_device
/sys/devices/software
/sys/class/firmware
/sys/firmware
/sys/module/firmware_class
bash-4.3# find / -name *.war -type f
/usr/share/jenkins/jenkins.war
bash-4.3# exit
$
```
![alt tag](https://i.imgur.com/vkGPLgj.png)  

*  docker cpコマンドでコンテナ内の該当するディレクトリー（2つ）をDockerホスト上に取得する  
```
$ mkdir -p .srv/jenkins/var/cache
$ mkdir -p .srv/jenkins/var/cache/jenkins/
$ docker cp jenkins_1:/var/cache/jenkins/war ./srv/jenkins/var/cache/jenkins/
$ docker cp jenkins_1:/usr/share/jenkins ./srv/jenkins/usr/share/
```


```
test@test-Ubuntu18:~/docker-jenkins-django-tutorial/jenkins_volume$ mkdir -p ./jenkins/var/cache
$ mkdir -p ./jenkins/var/cache/jenkins/
$ mkdir -p ./jenkins/usr
$ mkdir -p ./jenkins/usr/share/
$ docker cp jenkins_volume_jenkins_1:/var/jenkins_home/war ./jenkins/var/cache/jenkins/
$ docker cp jenkins_volume_jenkins_1:/usr/share/jenkins ./jenkins/usr/share/
```
![alt tag](https://i.imgur.com/SC14AtE.png)  

![alt tag](https://i.imgur.com/Yd4Teth.png)  

*  docker-compose.ymlを編集しjenkinsのコンテナの永続化ボリュームを2つ追加する  
```
./srv/jenkins/var/cache/jenkins/war:/var/cache/jenkins/war
./srv/jenkins/usr/share/jenkins:/usr/share/jenkins
```

*  docker-compose.yml on my site  
```
./jenkins/var/cache/jenkins/war:/var/jenkins_home/war
./jenkins/usr/share/jenkins:/usr/share/jenkins
```
![alt tag](https://i.imgur.com/DrbNF77.png)  


*  docker-compose up --build -dを実行  
*  ブラウザー上からjenkinsのアップグレードを実行  
*  docker-compose downを実行  
*  docker-compose up -dを実行  
*  jenkinsは正常に起動し、アップグレードしたバージョンが維持されている  

![alt tag](https://i.imgur.com/5CfCyK9.png)  

![alt tag](https://i.imgur.com/vg2uHzQ.png)  

![alt tag](https://i.imgur.com/G3asieQ.png)  

![alt tag](https://i.imgur.com/IVrOKM1.png)  

# Redmine+Jenkins+GitLab+Elasticsearch by Docker  
[docker-composeで開発環境全部盛りしてみた Mar 15, 2020](https://qiita.com/euledge/items/52eb2e299a128f528d42)  

## .env  
```

# Common settings
DNS1=xxx.xxx.xxx.xxx
DNS2=xxx.xxx.xxx.xxx

SMTP_DOMAIN=xxx.xxx.xxx.xxx
SMTP_HOST=mail.xxx.xxx.xxx
SMTP_PORT=25
SMTP_USER=
SMTP_PASS=

LDAP_LABEL=xxxxxx
LDAP_HOST=xxx.xxx.xxx.xxx
LDAP_VERIFY_SSL=false
LDAP_BIND_DN=CN=xxx,CN=xxx,DC=xxx,DC=xxx,DC=xxx,DC=xxx
LDAP_PASS=xxx
LDAP_ALLOW_USERNAME_OR_EMAIL_LOGIN=true
LDAP_BASE=DC=xxx,DC=xxx,DC=xxx,DC=xxx

HTTP_PROXY=http://proxy.xxx.xxx.xxx.xxx:8080
HTTPS_PROXY=http://proxy.xxx.xxx.xxx.xxx:8080
NO_PROXY=127.0.0.1,localhost
HTTP_PROXY_HOST=http://proxy.xxx.xxx.xxx.xxx
HTTP_PROXY_PORT=8080


# PlantUML settings
PLANTUML_PORT=xxxxx

# Nexus settings
NEXUS_PORT=xxxxx

# PostgreSQL settings
POSTGRESQL_VERSION=10-2

# GitLab settings
GITLAB_VERSION=12.7.6
GITLAB_DB_USER=gitlab
GITLAB_DB_PASS=password

GITLAB_HOST=xxx.xxx.xxx.xxx
GITLAB_PORT=xxxxx
GITLAB_SSH_PORT=xxxxx
GITLAB_RELATIVE_URL_ROOT=

GITLAB_EMAIL=gitlab@xxx.xxx.xxx.xxx
GITLAB_EMAIL_REPLY_TO=noreply@example.com
GITLAB_INCOMING_EMAIL_ADDRESS=gitlab@xxx.xxx.xxx.xxx
GITLAB_ROOT_PASSWORD=password

GITLAB_PROJECTS_ISSUES=true
GITLAB_PROJECTS_MERGE_REQUESTS=true
GITLAB_PROJECTS_WIKI=true
GITLAB_PROJECTS_SNIPPETS=true
GITLAB_PROJECTS_BUILDS=true
GITLAB_PROJECTS_CONTAINER_REGISTRY=true
GITLAB_PAGES_ENABLED=true
GITLAB_MATTERMOST_ENABLED=true

# Redmine settings
REDMINE_VERSION=4.1.0
REDMINE_DB_USER=redmine
REDMINE_DB_PASS=password

REDMINE_PORT=xxxxx

# Jenlins settings
JENKINS_PORT=xxxxx

# ElasticSearch settings
ES_VERSION=7.6.1
ES_PORT=9200
LOGSTASH_PORT=9600
GRAFANA_VERSION=latest
GRAFANA_PORT=xxxxx
ELASTICSEARCH_PROTO=http
ELASTICSEARCH_HOST=elasticsearch
ELASTICSEARCH_PORT=9200

# Rocketchat settings
ROCKETCHAT_VERSION=3.0.3
ROCKETCHAT_URL=http://xxx.xxx.xxx.xxx:yyyyy
ROCKETCHAT_PORT=yyyyy

HUBOT_PORT=xxxxx
```

## docker-compose.yml  
```

version: '2'

services:
# Settings for PlantUML
  plantuml:
    image: plantuml/plantuml-server:jetty
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
    ports:
      - "${PLANTUML_PORT}:8080"

  gitlab-redis:
    image: sameersbn/redis:latest
    command:
      - --loglevel warning
    volumes:
      - gitlab-redis-vol:/var/lib/redis:Z

  gitlab-db:
    image: sameersbn/postgresql:${POSTGRESQL_VERSION}
    volumes:
      - gitlab-db-vol:/var/lib/postgresql:Z
    environment:
      - DB_USER=${GITLAB_DB_USER}
      - DB_PASS=${GITLAB_DB_PASS}
      - DB_NAME=gitlabhq_production
      - DB_EXTENSION=pg_trgm

# Settings For Gitlab
  gitlab:
    image: sameersbn/gitlab:${GITLAB_VERSION}
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    depends_on:
      - gitlab-redis
      - gitlab-db
    ports:
      - "${GITLAB_PORT}:80"
      - "${GITLAB_SSH_PORT}:22"
    volumes:
      - gitlab-vol:/home/git/data:Z
      - gitlab-socket-vol:/var/run/docker.sock
    environment:
      - DEBUG=false
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}

      - DB_ADAPTER=postgresql
      - DB_HOST=gitlab-db
      - DB_PORT=5432
      - DB_USER=${GITLAB_DB_USER}
      - DB_PASS=${GITLAB_DB_PASS}
      - DB_NAME=gitlabhq_production

      - REDIS_HOST=gitlab-redis
      - REDIS_PORT=6379

      - TZ=Asia/Tokyo
      - GITLAB_TIMEZONE=Tokyo

      - GITLAB_HTTPS=false
      - SSL_SELF_SIGNED=false

      - GITLAB_HOST=${GITLAB_HOST}
      - GITLAB_PORT=${GITLAB_PORT}
      - GITLAB_SSH_PORT=${GITLAB_SSH_PORT}
      - GITLAB_RELATIVE_URL_ROOT=${GITLAB_RELATIVE_URL_ROOT}

      - GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alphanumeric-string
      - GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alphanumeric-string
      - GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alphanumeric-string

      - GITLAB_ROOT_PASSWORD=
      - GITLAB_ROOT_EMAIL=

      - GITLAB_NOTIFY_ON_BROKEN_BUILDS=true
      - GITLAB_NOTIFY_PUSHER=false

      - GITLAB_EMAIL=gitlab@example.com
      - GITLAB_EMAIL_REPLY_TO=noreply@example.com
      - GITLAB_INCOMING_EMAIL_ADDRESS=reply@example.com
      - GITLAB_SIGNUP_ENABLED=false
      - GITLAB_BACKUP_SCHEDULE=daily
      - GITLAB_BACKUP_EXPIRY=604800
      - GITLAB_BACKUP_TIME=01:00

      - SMTP_ENABLED=true
      - SMTP_DOMAIN=${SMTP_DOMAIN}
      - SMTP_HOST=${SMTP_HOST}
      - SMTP_PORT=${SMTP_PORT}
      - SMTP_USER=${SMTP_USER}
      - SMTP_PASS=${SMTP_PASS}
      - SMTP_STARTTLS=false
      - SMTP_AUTHENTICATION=false

      - LDAP_ENABLED=true
      - LDAP_LABEL=${LDAP_LABEL}
      - LDAP_HOST=${LDAP_HOST}
      - LDAP_VERIFY_SSL=${LDAP_VERIFY_SSL}
      - LDAP_BIND_DN=${LDAP_BIND_DN}
      - LDAP_PASS=${LDAP_PASS}
      - LDAP_ALLOW_USERNAME_OR_EMAIL_LOGIN=${LDAP_ALLOW_USERNAME_OR_EMAIL_LOGIN}
      - LDAP_BASE=${LDAP_BASE}

      - GITLAB_PROJECTS_ISSUES=${GITLAB_PROJECTS_ISSUES}
      - GITLAB_PROJECTS_MERGE_REQUESTS=${GITLAB_PROJECTS_MERGE_REQUESTS}
      - GITLAB_PROJECTS_WIKI=${GITLAB_PROJECTS_WIKI}
      - GITLAB_PROJECTS_SNIPPETS=${GITLAB_PROJECTS_SNIPPETS}
      - GITLAB_PROJECTS_BUILDS=${GITLAB_PROJECTS_BUILDS}
      - GITLAB_PAGES_ENABLED=${GITLAB_PAGES_ENABLED}
      - GITLAB_MATTERMOST_ENABLED=${GITLAB_MATTERMOST_ENABLED}
  gitlab-runner:
    image: gitlab/gitlab-runner:latest
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
    volumes:
      - gitlab-runner-config-vol:/etc/gitlab-runner
      - gitlab-socket-vol:/var/run/docker.sock
# Redmineの設定
  redmine-db:
    image: sameersbn/postgresql:${POSTGRESQL_VERSION}
    restart: unless-stopped
    environment:
      - DB_USER=${REDMINE_DB_USER}
      - DB_PASS=${REDMINE_DB_PASS}
    volumes:
      - redmine-db-vol:/var/lib/postgresql
  redmine:
    image: sameersbn/redmine:${REDMINE_VERSION}
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    depends_on:
      - redmine-db
    environment:
      - TZ=Asia/Tokyo
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}

      - DB_ADAPTER=postgresql
      - DB_HOST=redmine-db
      - DB_PORT=5432
      - DB_USER=${REDMINE_DB_USER}
      - DB_PASS=${REDMINE_DB_PASS}
      - DB_NAME=redmine_production

      - REDMINE_PORT=${REDMINE_PORT}
      - REDMINE_HTTPS=false
      - REDMINE_RELATIVE_URL_ROOT=
      - REDMINE_SECRET_TOKEN=

      - REDMINE_SUDO_MODE_ENABLED=false
      - REDMINE_SUDO_MODE_TIMEOUT=15

      - REDMINE_CONCURRENT_UPLOADS=2

      - REDMINE_BACKUP_SCHEDULE=daily
      - REDMINE_BACKUP_EXPIRY=604800
      - REDMINE_BACKUP_TIME=02:00

      - SMTP_ENABLED=true
      - SMTP_DOMAIN=${SMTP_DOMAIN}
      - SMTP_HOST=${SMTP_HOST}
      - SMTP_PORT=${SMTP_PORT}
      - SMTP_USER=${SMTP_USER}
      - SMTP_PASS=${SMTP_PASS}
      - SMTP_STARTTLS=false
      - SMTP_AUTHENTICATION=false

      - LDAP_ENABLED=true
      - LDAP_LABEL=${LDAP_LABEL}
      - LDAP_HOST=${LDAP_HOST}
      - LDAP_VERIFY_SSL=${LDAP_VERIFY_SSL}
      - LDAP_BIND_DN=${LDAP_BIND_DN}
      - LDAP_PASS=${LDAP_PASS}
      - LDAP_ALLOW_USERNAME_OR_EMAIL_LOGIN=${LDAP_ALLOW_USERNAME_OR_EMAIL_LOGIN}
      - LDAP_BASE=${LDAP_BASE}

    ports:
      - "${REDMINE_PORT}:80"
    volumes:
      - redmine-vol:/home/redmine/data:Z
      - gitlab-vol:/home/git/data:ro

# Settings for nexus
  nexus:
    image: sonatype/nexus3
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    ports:
      - "${NEXUS_PORT}:8081"
    volumes:
      - nexus-vol:/nexus-data:Z
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
      - JAVA_OPTS=-Duser.timezone=Asia/Tokyo -Dfile.encoding=UTF-8 -Dsun.jnu.encoding=UTF-8

# Settings for Jenkins
  jenkins:
    image: jenkinsci/blueocean
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    user: root
    ports:
      - '${JENKINS_PORT}:8080'
    volumes:
      - jenkins-vol:/var/jenkins_home:Z
      - /etc/localtime:/etc/localtime:ro
      - /etc/docker:/etc/docker:ro
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - JAVA_OPTS=-Duser.timezone=Asia/Tokyo -Dfile.encoding=UTF-8 -Dsun.jnu.encoding=UTF-8
      - TZ=Asia/Tokyo
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}

# Settings for ELK
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:${ES_VERSION}
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}

      - discovery.type=single-node
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    ports:
      - ${ES_PORT}:9200
    volumes:
      - elastic-vol:/usr/share/elasticsearch/data

  logstash:
    image: docker.elastic.co/logstash/logstash:${ES_VERSION}
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
      - discovery.type=single-node
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    depends_on:
      - elasticsearch
    ports:
      - ${LOGSTASH_PORT}:9600
    volumes:
      - logstash-vol:/usr/share/logstash

  grafana:
    image: grafana/grafana:${GRAFANA_VERSION}
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
    depends_on:
      - elasticsearch
    ports:
      - ${GRAFANA_PORT}:3000
    volumes:
      - grafana-vol:/var/lib/grafana

# Settings for Communications
  mongo:
    image: mongo:4.0.16
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
      - TZ=Asia/Tokyo
    volumes:
      - rocketchat-db-vol:/data
      - /etc/localtime:/etc/localtime:ro
    command: mongod --smallfiles --oplogSize 128 --replSet rs0 --storageEngine=mmapv1

  # initialization mongodb for create replicaset no need restart
  mongoinitreplica:
    image: mongo:4.0.16
    depends_on:
      - mongo
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
      - TZ=Asia/Tokyo
    volumes:
      - /etc/localtime:/etc/localtime:ro
    command: 'mongo mongo/rocketchat --eval "rs.initiate({ _id: ''rs0'', members: [ { _id: 0, host: ''mongo:27017'' } ]})"'

  rocketchat:
    image: rocketchat/rocket.chat:${ROCKETCHAT_VERSION}
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    volumes:
      - rocketchat-vol:/app/uploads
      - /etc/localtime:/etc/localtime:ro
    depends_on:
      - mongoinitreplica
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
      - ROOT_URL=${ROCKETCHAT_URL}
      - MONGO_URL=mongodb://mongo:27017/rocketchat
      - MONGO_OPLOG_URL=mongodb://mongo:27017/local?replSet=rs0
      - TZ=Asia/Tokyo
    ports:
      - ${ROCKETCHAT_PORT}:3000

  hubot:
    image: rocketchat/hubot-rocketchat:latest
    restart: unless-stopped
    dns:
      - ${DNS1}
      - ${DNS2}
    environment:
      - HTTP_PROXY=${HTTP_PROXY}
      - HTTPS_PROXY=${HTTPS_PROXY}
      - NO_PROXY=${NO_PROXY}
      - ROCKETCHAT_URL=rocketchat:${ROCKETCHAT_PORT}
      - ROCKETCHAT_ROOM=GENERAL
      - ROCKETCHAT_USER=bot
      - ROCKETCHAT_PASSWORD=password
      - BOT_NAME=bot
      - EXTERNAL_SCRIPTS=hubot-help,hubot-seen,hubot-links,hubot-diagnostics,hubot-proxy-loader
      - TZ=Asia/Tokyo
    depends_on:
      - rocketchat
    labels:
      - "traefik.enable=false"
    volumes:
      - hubot-vol:/home/hubot
      - /etc/localtime:/etc/localtime:ro
    ports:
      - ${HUBOT_PORT}:8080

volumes:
  gitlab-redis-vol:
  gitlab-db-vol:
  gitlab-vol:
  gitlab-socket-vol:
  gitlab-runner-config-vol:
  redmine-db-vol:
  redmine-vol:
  nexus-vol:
  jenkins-vol:
  logstash-vol:
  elastic-vol:
  grafana-vol:
  rocketchat-db-vol:
  rocketchat-vol:
  hubot-vol:
```


# Troubleshooting


# Reference  

## Android App Automation Test  
[從0開始，全方面自動化測試Android App 2019-09-16](https://ithelp.ithome.com.tw/users/20120975/ironman/2726?page=1)  

## Jenkins for Android   
[[Android 十全大補] Jenkins 2019-10-13](https://ithelp.ithome.com.tw/articles/10227688)  



## GitLab, Redmine and Jenkins by Docker-Compose  
[Docker-ComposeでGitLabとRedmineとJenkinsを立ち上げる updated at 2017-08-29](https://qiita.com/nexkeh/items/02a4d6c33d884bda1b23)  
```
docker-compose.yml

gitlab-mysql:
  restart: always
  image: sameersbn/mysql:latest
  environment:
    - DB_USER=gitlab
    - DB_PASS=password
    - DB_NAME=gitlabhq_production
  volumes:
    - /srv/docker/gitlab/mysql:/var/lib/mysql
gitlab:
  restart: always
  image: sameersbn/gitlab:8.3.2
  links:
    - gitlab-redis:redisio
    - gitlab-mysql:mysql
  environment:
    - DEBUG=false
    - TZ=Asia/Tokyo
    - GITLAB_TIMEZONE=Tokyo

    - GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alphanumeric-string

    - GITLAB_HOST=
    - GITLAB_PORT=
    - GITLAB_SSH_PORT=
    - GITLAB_RELATIVE_URL_ROOT=/gitlab

    - GITLAB_NOTIFY_ON_BROKEN_BUILDS=true
    - GITLAB_NOTIFY_PUSHER=false

    - GITLAB_EMAIL=notifications@example.com
    - GITLAB_EMAIL_REPLY_TO=noreply@example.com
    - GITLAB_INCOMING_EMAIL_ADDRESS=reply@example.com

    - GITLAB_BACKUP_SCHEDULE=daily
    - GITLAB_BACKUP_TIME=01:00

    - SMTP_ENABLED=false
    - SMTP_DOMAIN=www.example.com
    - SMTP_HOST=smtp.gmail.com
    - SMTP_PORT=587
    - SMTP_USER=mailer@example.com
    - SMTP_PASS=password
    - SMTP_STARTTLS=true
    - SMTP_AUTHENTICATION=login

    - IMAP_ENABLED=false
    - IMAP_HOST=imap.gmail.com
    - IMAP_PORT=993
    - IMAP_USER=mailer@example.com
    - IMAP_PASS=password
    - IMAP_SSL=true
    - IMAP_STARTTLS=false
  volumes:
    - /srv/docker/gitlab/gitlab:/home/git/data
gitlab-redis:
  restart: always
  image: sameersbn/redis:latest
  volumes:
    - /srv/docker/gitlab/redis:/var/lib/redis
redmine-mysql:
  restart: always
  image: sameersbn/mysql:latest
  environment:
    - DB_USER=redmine
    - DB_PASS=password
    - DB_NAME=redmine_production
  volumes:
    - /srv/docker/redmine/mysql:/var/lib/mysql
redmine:
  restart: always
  image: sameersbn/redmine:latest
  links:
    - redmine-mysql:mysql
  environment:
    - TZ=Asia/Tokyo

    - REDMINE_PORT=
    - REDMINE_HTTPS=false
    - REDMINE_RELATIVE_URL_ROOT=/redmine
    - REDMINE_SECRET_TOKEN=

    - REDMINE_SUDO_MODE_ENABLED=false
    - REDMINE_SUDO_MODE_TIMEOUT=15

    - REDMINE_CONCURRENT_UPLOADS=2

    - REDMINE_BACKUP_SCHEDULE=
    - REDMINE_BACKUP_EXPIRY=
    - REDMINE_BACKUP_TIME=

    - SMTP_ENABLED=false
    - SMTP_METHOD=smtp
    - SMTP_DOMAIN=www.example.com
    - SMTP_HOST=smtp.gmail.com
    - SMTP_PORT=587
    - SMTP_USER=mailer@example.com
    - SMTP_PASS=password
    - SMTP_STARTTLS=true
    - SMTP_AUTHENTICATION=:login

    - IMAP_ENABLED=false
    - IMAP_HOST=imap.gmail.com
    - IMAP_PORT=993
    - IMAP_USER=mailer@example.com
    - IMAP_PASS=password
    - IMAP_SSL=true
    - IMAP_INTERVAL=30
  volumes:
    - /srv/docker/redmine/redmine:/home/redmine/data
jenkins:
  restart: always
  image: jenkins:latest
  environment:
    - JENKINS_OPTS=--prefix=/jenkins
  volumes:
    - /srv/docker/jenkins/jenkins:/var/jenkins_home
proxy:
  build: proxy
  links:
    - gitlab:gitlab
    - redmine:redmine
    - jenkins:jenkins
  ports:
    - "80:80"
```
> nginx用のDockerfile。docker-compose.ymlのproxyの部分で、このDockerfileを使ってnginxのイメージがビルドされます。  

```
Dockerfile

# https://registry.hub.docker.com/_/nginx/
FROM nginx:latest

# 設定ファイルをコピー
COPY nginx.conf /etc/nginx/nginx.conf

# HTMLファイルをコピー
COPY index.html /usr/share/nginx/html/index.html
```

### nginx.conf  
```
nginx.conf

user  nginx;
worker_processes  2;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
  worker_connections  1024;
}

http {
  include  /etc/nginx/mime.types;
  default_type  application/octet-stream;

  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
  access_log  /var/log/nginx/access.log  main;

  sendfile           on;
  keepalive_timeout  65;
  server_tokens      off;

  server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name 127.0.0.1;
    charset koi8-r;

    client_max_body_size 1024M;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto http;
    proxy_set_header X-Forwarded-Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-NginX-Proxy true;

    # static-html
    location / {
      index index.html;
      root /usr/share/nginx/html;
    }
    # gitlab
    location /gitlab {
      proxy_pass http://gitlab;
    }
    # redmine
    location /redmine {
      proxy_pass http://redmine;
    }
    # jenkins
    location /jenkins {
      proxy_pass http://jenkins:8080;
    }
  }
}
```

## Docker上に開発環境一式を構築する  
[Docker上に開発環境一式を構築する updated at 2016-04-07](https://qiita.com/hakaicode/items/5522a5ad1c280b9cf757)  
```
現在のステータス

    VPSにホストにUbuntuを入れる <-イマココ
    dnsを構築する
    nginxを構築する
    GitBucket環境を構築する
    Selenium用のJenkins環境を構築する
    Desktop環境(IntelliJ IDEA)を構築する
    まとめと反省
```

## Jenkinsを手探りで社内ローカルに立てて詰んだ話  
[Jenkinsを手探りで社内ローカルに立てて詰んだ話 Dec 14, 2017](https://qiita.com/wryu/items/de3767f231e690fb4e7d)  

## JenkinsとSeleniumを使ってWebコンテンツの自動UIテスト環境を作ろう！  
[JenkinsとSeleniumを使ってWebコンテンツの自動UIテスト環境を作ろう！ 4/9, 2015](https://ics.media/entry/6031/)  

## JenkinsでCI環境構築チュートリアル ～GitHubとの連携～  
[JenkinsでCI環境構築チュートリアル ～GitHubとの連携～ 12/9, 2016](https://ics.media/entry/2869/)  

## JenkinsでCI環境構築チュートリアル ～GitHubからWebサーバーへのデプロイ～  
[JenkinsでCI環境構築チュートリアル ～GitHubからWebサーバーへのデプロイ～ 11/14, 2015](https://ics.media/entry/3283/)  

## テスト自動化環境構築  
[テスト自動化環境構築  Jul 14, 2018](https://qiita.com/jun2014/items/0c97f99e60109f9ec870)  

## dockerでjenkins構築（plugin install errorを出さない）  
[dockerでjenkins構築（plugin install errorを出さない） Jul 08, 2019](https://qiita.com/_ainosh_/items/04992adbab8502e2ed9e)  
* 1. 適当にjenkins試す用のディレクトリ作って、docker-compose.yml作成する
docker-compose.yml

```
version: "3"
services:
  master:
    container_name: master
    # (library/)jenkins:2.60.3（公式）だと依存プラグインの関係でインストールがエラるので、jenkins/jenkins使う
    image: jenkins/jenkins:lts
    ports:
      - 18080:8080
      - 50000:50000
    volumes:
      - ./jenkins_home:/var/jenkins_home
#    links:
#     - slave01
#
#  slave01:
#    container_name: slave01
#    image: jenkinsci/ssh-slave
#    environment:
#      - JENKINS_SLAVE_SSH_PUBKEY=ssh-rsa AAAxxxxxxxxxxxx
```

```
docker-compose up --build -d
```

```
docker logs master
```

* 2. master側のSSHキー作成  
```
docker exec -it master ssh-keygen -t rsa -C "" 
```


* 3. slaveを作成する  
docker-compose.yml

```

version: "3"
services:
  master:
    container_name: master
    # (library/)jenkins:2.60.3（公式）だと依存プラグインの関係でインストールがエラるので、jenkins/jenkins使う
    image: jenkins/jenkins:lts
    ports:
      - 18080:8080
      - 50000:50000
    volumes:
      - ./jenkins_home:/var/jenkins_home
    links:
     - slave01

  slave01:
    container_name: slave01
    image: jenkinsci/ssh-slave
    environment:
      - JENKINS_SLAVE_SSH_PUBKEY=ssh-rsa AAAAxxxxxxxxxxxxxxxx
```
```
docker-compose up --build -d
```

* 4. 適当にjenkinsの画面でポチポチ設定してください  
http://localhost:18080/

秘密鍵の入力を[Jenkinsのマスター上の~/.sshから]にしたいんですが、ないので直接入力しました。
![alt tag](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F455534%2F64551bb3-ae7d-caf7-abc0-592eff8a183e.png?ixlib=rb-1.2.2&auto=compress%2Cformat&gif-q=60&w=1400&fit=max&s=1c0de9750426a6584da9e338a7718474)  



[docker-composeを使って開発環境でjenkinsを動かせるようにする Oct 31, 2018](https://qiita.com/i_whammy_/items/84b71c56d70817803472)  

## Jenkinsインストール(Ansible)  
[Jenkinsインストール(Ansible) Oct 24, 2019](https://qiita.com/tz2i5i_ebinuma/items/c7cdb0520062a6c75685)  

## 【Jenkins備忘録】Python自動テスト環境構築  
[【Jenkins備忘録】Python自動テスト環境構築①Python準備編  Jun 09, 2018](https://qiita.com/Kento75/items/8558d9ddd2cb04c3e36d)  
[【Jenkins備忘録】Python自動テスト環境構築②テストコード準備編 Jun 09, 2018](https://qiita.com/Kento75/items/e4ebeb990449f447ce3f)  
[【Jenkins備忘録】Python自動テスト環境構築③jenkins準備編 Jun 09, 2018](https://qiita.com/Kento75/items/2ecd1f3251c9c344ca69)  

[【Jenkins備忘録】Python自動テスト環境構築④プロジェクト作成編 Jun 09, 2018](https://qiita.com/Kento75/items/f46b4c47a3a33de7ae5d)  

## Dockerで動くJenkinsから他のコンテナを操作する（Docker outside of Docker） 
* [Dockerで動くJenkinsから他のコンテナを操作する（Docker outside of Docker） Oct 26, 2019](https://qiita.com/quotto/items/61b44bdeef7dbb915970)  

## Dockerでjenkins(+SSL)  
* [Dockerでjenkins(+SSL) Sep 05, 2019](https://qiita.com/oko1977/items/8b2f782bb7ead8e27659)  

## Jenkins CheatSheet — Know The Top Best Practices of Jenkins  
[Jenkins CheatSheet — Know The Top Best Practices of Jenkins Aug 7, 2019](https://medium.com/edureka/jenkins-cheat-sheet-e0f7e25558a3)  
```
Different Types of Jenkins Jobs

Below are the types of jobs you can choose from:

    Freestyle

Freestyle build jobs are general-purpose build jobs, which provides maximum flexibility. It can be used for any type of project.

    Pipeline

This project runs the entire software development workflow as code. Instead of creating several jobs for each stage of software development, you can now run the entire workflow as one code.

    Multiconfiguration

The multiconfiguration project allows you to run the same build job on different environments. It is used for testing an application in different environments.

    Folder

This project allows users to create folders to organize and categorize similar jobs in one folder or subfolder.

    GitHub Organization

This project scans your entire GitHub organization and creates Pipeline jobs for each repository containing a Jenkinsfile

    Multibranch Pipeline

This project type lets you implement different Jenkinsfiles for different branches of the same project.
```

## Devops实践中的CICD工具  
[Devops实践中的CICD工具](https://mp.weixin.qq.com/s/r7CnMAIVJ_gMLL8WppWZbw)

* []()  
## 
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




