---
layout: post
title: "Vagrantによるcoreosでdockerのprovisioning (まだ失敗)"
date: 2014-01-14 23:51:17 +0900
comments: true
categories: 
---
dockerによるprovisioningだとメッセージがでなくて不安になってくるので(実際時間がかかっているし)shell provisionならば情報が得られるかと思い試してみる。その前提としてshared folderを使えるようにしなくては。

coreosのVagrantfileのREADME.mdにshared folderの方法が書いてあったので、試してみた。

```
config.vm.network "private_network", ip:"172.12.8.150"
config.vm.synced_folder ".", "/home/core/share", id: "core", :nfs => true, :mount_options => ['nolock,vers=3,udp']

```

でも以下のメッセージが出てきて失敗。

```
There are errors in the configuration of this machine. Please fix
the following errors and try again:

vm:
* It appears your machine doesn't support NFS, or there is not an
adapter to enable NFS on this machine for Vagrant. Please verify
that `nfsd` is installed on your machine, and try again. If you're
on Windows, NFS isn't supported. If the problem persists, please
contact Vagrant support.
```

ホストマシンにはnfsがセットアップされていなかったかも。でvagrantにお任せしてみました。

```
  # shared folder
  config.vm.synced_folder ".", "/home/core/share"
```

どうもtimeoutになるようなので

```
vagrant box add coreos http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_vagrant.box
vagrant up
```

で試してみたらcoreosは起動した模様。しかし、shared folderは

```
[default] Machine booted and ready!
[default] No guest additions were detected on the base box for this VM! Guest
additions are required for forwarded ports, shared folders, host only
networking, and more. If SSH fails on this machine, please install
the guest additions and repackage the box to continue.

This is not an error message; everything may continue to work properly,
in which case you may ignore this message.
[default] Mounting shared folders...
[default] -- /home/core/share
Failed to mount folders in Linux guest. This is usually beacuse
the "vboxsf" file system is not available. Please verify that
the guest additions are properly installed in the guest and
can work properly. The command attempted was:

mount -t vboxsf -o uid=`id -u core`,gid=`getent group core | cut -d: -f3` /home/core/share /home/core/share
mount -t vboxsf -o uid=`id -u core`,gid=`id -g core` /home/core/share /home/core/share
```

となり、失敗。Guest additionsなるものが足りないようだけど。coreosにはGuest additionがないのか。。？nfsにすればよかったかな。。まだよくわからないので今日はここまで。

2014/1/17追記: vagrant box add coreos URL と書いてあった箇所の```URL```を使ったURLに加筆修正しました。