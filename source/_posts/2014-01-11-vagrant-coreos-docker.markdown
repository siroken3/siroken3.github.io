---
layout: post
title: "vagrant+coreos+dockerをやってみた"
date: 2014-01-11 00:59:37 +0900
comments: true
categories: 
---
自宅のマシンがubuntu 13.10の32bit版だったのでdockerが使えなくてあきらめかけていたけれど、VirtualBoxで64bitマシンを動かせばいいのだと思ったら俄然気になって、調べてたらVagrantでboxを作るのが楽そうということが分かった。<!-- more -->Vagrantは以前同僚が使っていたけれどよく分かってなくて放置してたのだけど楽に64bit仮想マシンが作れるので飛びついた次第。(もっと早く試していればよかった)

Vagrantファイルを作ってそのディレクトリでvagrant upで仮想マシン起動、vagrant sshでその仮想マシンへssh接続。楽だった。
Vagrantのprovision機能にdockerが追加されたのでこれまでvagrant ssh したあとやホストマシンと仮想マシンの共有ディレクトリにDockerfileをおいてdocker buildさせていたのが Vagrant upで全て行える模様。ちょっと今日のところは眠いのでブックマーク代わりにリンクを貼っておく。

* http://iakio.hatenablog.com/entry/2013/11/28/002657
* http://docs.vagrantup.com/v2/provisioning/docker.html
* https://index.docker.io/u/fujin/influxdb/
* http://shibayu36.hatenablog.com/entry/2013/12/07/233510
* http://coreos.com/blog/coreos-vagrant-images/

今日はInfluxDBが入ったdockerイメージをcoreosが起動した仮想マシンで稼働させ、ホストマシンから接続することまでできた。ホストマシン -> 仮想マシン -> docker の2段port forwardがキー。Vagrantには仮想マシンのポートをホストマシンにそのままFORWARDする機能があるように読めたが、どうも仮想マシンにprivate ipを付与することで会社のMacbookProでは成功するようだった。またdockerのポートフォワードは49000番以降になってることに注意が必要だった。
