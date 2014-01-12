---
layout: post
title: "dockerのubuntu image へnginxをインストール少し前進"
date: 2014-01-12 23:19:16 +0900
comments: true
categories: 
---
昨日dockerのprovisioningでtimeoutになった原因を調べようと,vagrant sshでcoreosに入り
```
docker ps -a
```
してみたら、apt-get install -u openssh-server nginx supervisorでexit 100で失敗していた。
どうも、nginxをインストールする時に http://wiki.nginx.org/InstallJa にかかれているようにPPAを登録しないとnginxはインストールできなかった模様。
