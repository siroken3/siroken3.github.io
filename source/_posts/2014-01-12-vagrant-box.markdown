---
layout: post
title: "dockerでprovisionを欲張ってvagrant upでtimeout"
date: 2014-01-12 00:06:19 +0900
comments: true
categories: 
---
Vagrantfileでdockerによるprovisionができるとのことで以下のようにしたらタイムアウトになってしまった。provision後にDockerfile作ったほうがよいのか。。途中経過がわからないままtimeoutになるのはしんどい。
```
  config.vm.provision "docker" do |d|
    d.pull_images ["ubuntu"]
    d.run "ubuntu", cmd: "apt-get update"
    d.run "ubuntu", cmd: "apt-get upgrade -y"
    d.run "ubuntu", cmd: "apt-get install -y openssh-server nginx supervisor"
  end
```
