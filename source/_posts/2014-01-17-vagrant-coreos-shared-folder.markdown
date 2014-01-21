---
layout: post
title: "Vagrantによるcoreosでdockerのprovisioning(まだまだ失敗)"
date: 2014-01-17 01:54:32 +0900
comments: true
categories: docker
---
全回のpostでdockerのprovisionをするために以下の道のりをたどっています。

 * Dockerfileをhost machineに用意
 * それをshared folderで共有
   * nfsがhost machineになかったのでVagrantお任せしてみた
     * ここでつまずいてる
 * guest machineのshell provisioningでdockerのrun

ヤク毛刈りもいい加減にしないと。<!-- more -->ひとまずShared Folderをなんとかしようとエラーメッセージに従いhost machine(ubuntu 13.10)にnfsをインストールしました。

```
$ sudo aptitude install nfs-kernel-server
```

で```vagrant reload```。

```
[default] Attempting graceful shutdown of VM...
[default] Clearing any previously set forwarded ports...
[default] Clearing any previously set network interfaces...
[default] Preparing network interfaces based on configuration...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] Running 'pre-boot' VM customizations...
[default] Booting VM...
[default] Waiting for machine to boot. This may take a few minutes...
[default] Machine booted and ready!
[default] No guest additions were detected on the base box for this VM! Guest
additions are required for forwarded ports, shared folders, host only
networking, and more. If SSH fails on this machine, please install
the guest additions and repackage the box to continue.

This is not an error message; everything may continue to work properly,
in which case you may ignore this message.
[default] Configuring and enabling network interfaces...
No guest IP was given to the Vagrant core NFS helper. This is an
internal error that should be reported as a bug.
```
うーん。bugですか・・? vagrant 1.4.0を使っていたのですが1.4.3がリリースされていたのでvagrant.comからdebパッケージファイルを入手してインストール。(Changelogとか見たほうがいいのでは。。)

```
sudo dpkg -i vagrant_1.4.3_i686.deb

```

そして一旦boxを削除してから再度挑戦。

```
$ vagrant box remove coreos
$ vagrant box add coreos http://storage.core-os.net/coreos/amd64-generic/dev-channel/coreos_production_vagrant.box
$ vagrant up
$[default] Configuring and enabling network interfaces...
[default] Exporting NFS shared folders...
Preparing to edit /etc/exports. Administrator privileges will be required...
[sudo] password for siroken3: 
nfsd not running
 * Exporting directories for NFS kernel daemon...                                            [ OK ] 
 * Starting NFS kernel daemon                                                                [ OK ] 
[default] Mounting NFS shared folders...
[default] VM already provisioned. Run `vagrant provision` or use `--provision` to force it

```

なんかいい感じだー。そこで起動したcoreos box へsshでログイン。

```
$ vagrant ssh
Last login: Thu Jan 16 17:20:42 UTC 2014 from 10.0.2.2 on ssh
   ______                ____  _____
  / ____/___  ________  / __ \/ ___/
 / /   / __ \/ ___/ _ \/ / / /\__ \
/ /___/ /_/ / /  /  __/ /_/ /___/ /
\____/\____/_/   \___/\____//____/
core@localhost ~ $ ls -la
total 24
drwxr-xr-x 4 core core 4096 Jan 14 15:51 .
drwxr-xr-x 3 root root 4096 Jan 12 16:01 ..
-rw------- 1 core core 2113 Jan 16 16:43 .bash_history
drwx------ 3 core core 4096 Jan 16 17:20 .ssh
-rw-r--r-- 1 core core  343 Jan 13 15:20 Dockerfile
drwxr-xr-x 5 core core 4096 Jan 16 16:43 share
core@localhost ~ $ cd share/
core@localhost ~/share $ ls
README.md  Vagrantfile  cluster
```

おぉ、~/share 以下にhost machineのファイル達が見える!共有成功!ちょっと進んで気分が良いので今日はこれまで。

