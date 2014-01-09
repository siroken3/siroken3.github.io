---
layout: post
title: "redis config の keep-alive"
date: 2014-01-09 23:56:23 +0900
comments: true
categories: redis
---
redisのconfigにkeep-aliveというのがある。クライアントが突然shutdownしたときにこれをサーバ側で検知するるにはkeep-aliveに適切な値を設定する必要がある(0だと無効)。推奨値は60。古いconnectionがずっと残っていたのをなんとかするために、同僚と検証。

timeoutとい項目もあるがtwemproxyを使っている場合、timeoutによってkillされたコネクションは切られてしまい(再接続できない)、redisの再起動が必要なようだ。