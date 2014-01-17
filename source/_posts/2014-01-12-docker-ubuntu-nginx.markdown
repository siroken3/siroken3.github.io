---
layout: post
title: "dockerのubuntu image へnginxをインストール試行錯誤"
date: 2014-01-12 23:19:16 +0900
comments: true
categories: 
---
昨日dockerのprovisioningでtimeoutになった原因を調べようと,vagrant sshでcoreosに入り
```
docker ps -a
```
してみたら、apt-get install -u openssh-server nginx supervisorでexit 100で失敗していた。<!-- more -->

nginxをインストールするには、以下のようにする必要がある。
http://wiki.nginx.org/InstallJa

なので、この手順のようにVagrantのprovisionを書く必要がありそう。

また、vagrant destroy で一旦クリーンな状態にしてubuntuイメージをvagrant のprovisionで取得させようとしたところやはりtimeoutになった。こちらは

```
Pulling repository ubuntu
8xxxxxxxxxxx: Downloading [=============================================>     ] 52.92 MB/58.34 MB 1m1s
b750fe79269d: Pulling dependent layers 
2yyyyyyyyyyy: Downloading [============================>                      ] 54.59 MB/94.86 MB 7m19s
```
と、ubuntuイメージの取得に結構時間がかかっている模様。(これはだいぶ経過してからのキャプチャ。最初は17minが表示されていた)

```
config.vm.boot_timeout = 3600
```
などにした方がよさそうだけど、時間がかかり過ぎてる気が。。実運用にはprivate repositoryがよいんだろうな。ひとまずprovisionせずにDockerfileでFROM ubuntuして取得するところから。

