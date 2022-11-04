---
title: EMQX
date: 2022-11-04 15:29:36
tags:
---

## EMQ X R3.2 消息服务器简介

EMQ X (Erlang/Enterprise/Elastic MQTT Broker) 是基于 Erlang/OTP 平台开发的开源物联网 MQTT 消息服务器。Erlang/OTP 是出色的软实时(Soft-Realtime)、低延时(Low-Latency)、分布式(Distributed) 的语言。MQTT 是轻量的(Lightweight)、发布订阅模式(PubSub) 的物联网消息协议。

<!--more-->

EMQ X 面向海量的 **移动/物联网/车载** 等终端接入，并实现在海量物理网设备间快速低延时的消息路由:

1. 稳定承载大规模的 MQTT 客户端连接，单服务器节点支持百万连接。
2. 分布式节点集群，快速低延时的消息路由，单集群支持千万规模的路由。
3. 消息服务器内扩展，支持定制多种认证方式、高效存储消息到后端数据库。
4. 完整物联网协议支持，MQTT、MQTT-SN、CoAP、LwM2M、私有 TCP/UDP 协议支持。

## MQTT 发布订阅模式简述

MQTT 是基于 **发布(Publish)/订阅(Subscribe)** 模式来进行通信及数据交换的，与 HTTP 的 **请求(Request)/应答(Response)** 的模式有本质的不同。

**订阅者(Subscriber)** 会向 **消息服务器(Broker)** 订阅一个 **主题(Topic)** 。成功订阅后，消息服务器会将该主题下的消息转发给所有的订阅者。

主题(Topic)以 '/' 为分隔符区分不同的层级。包含通配符 '+' 或 '#' 的主题又称为 **主题过滤器(Topic Filters)** ; 不含通配符的称为 **主题名(Topic Names)** 例如:

注: '+' 通配一个层级，'#' 通配多个层级(必须在末尾)。 注: 发布者(Publisher) 只能向 '主题名' 发布消息，订阅者(Subscriber) 则可以通过订阅 '主题过滤器' 来通配多个主题名称。

## MQTT 发布订阅

MQTT 是为移动互联网、物联网设计的轻量发布订阅模式的消息服务器，目前支持 MQTT [v3.1.1](http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html)和 [v5.0](http://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html):

![MQTT pub/sub](/pic/mqtt-pubsub-2022-11-02-1413.png)

EMQ X 启动后，任何设备或终端可通过 MQTT 协议连接到服务器，通过 **发布(Publish)/订阅(Subscribe)** 进行交换消息。

 [MQTT 客户端库](https://github.com/mqtt/mqtt.github.io/wiki/libraries)

例如，mosquitto_sub/pub 命令行发布订阅消息:

```
mosquitto_sub -h 127.0.0.1 -p 1883 -t topic -q 2
mosquitto_pub -h 127.0.0.1 -p 1883 -t topic -q 1 -m "Hello, MQTT!"
```

## 认证/访问控制

**EMQ X** 消息服务器 *连接认证* 和 *访问控制* 由一系列的认证插件(Plugins)提供，他们的命名都符合 `emqx_auth_\<name>` 的规则。

在 EMQ X 中，这两个功能分别是指：

1. **连接认证** : EMQ X 校验每个连接上的客户端是否具有接入系统的权限，若没有则会断开该连接
2. **访问控制** : EMQ X 校验客户端每个 *发布(Publish)/订阅(Subscribe)* 的权限，以 *允许/拒绝* 相应操作

### 认证(Authentication)

EMQ X 消息服务器认证由一系列认证插件(Plugins)提供，系统支持按用户名密码、ClientID 或匿名认证。

系统默认开启匿名认证(Anonymous)，通过加载认证插件可开启的多个认证模块组成认证链:

![the-certification-chain](/pic/the-certification-chain_2022-11-04.png)

**开启匿名认证**

etc/emqx.conf 配置启用匿名认证:

```
允许匿名访问
## Value: true | false
allow_anonymous = true
```

### 访问控制(ACL)

EMQ X 消息服务器通过 ACL(Access Control List) 实现 MQTT 客户端访问控制。

ACL 访问控制规则定义:

```
允许(Allow)|拒绝(Deny) 谁(Who) 订阅(Subscribe)|发布(Publish) 主题列表(Topics)
```

MQTT 客户端发起订阅/发布请求时，EMQ X 消息服务器的访问控制模块会逐条匹配 ACL 规则，直到匹配成功为止:

![ACL](/pic/ACL-20221104.png)

## 共享订阅 (Shared Subscription)

EMQ X R3.0 版本开始支持集群级别的共享订阅功能。共享订阅(Shared Subscription)支持多种消息派发策略:

![Shared-subscription](/pic/Shared-subscription-202211041810.png)

共享订阅支持两种使用方式:

| 订阅前缀            | 使用示例                                  |
| --------------- | ------------------------------------- |
| $queue/         | mosquitto_sub -t '$queue/topic'       |
| $share/<group>/ | mosquitto_sub -t '$share/group/topic' |

示例:

```
mosquitto_sub -t '$share/group/topic'

mosquitto_pub -t 'topic' -m msg -q 2
```

EMQ X 通过 etc/emqx.conf 中的 broker.shared_subscription_strategy 字段配置共享消息的派发策略。

目前支持按以下几种策略派发消息：

| 策略          | 说明              |
| ----------- | --------------- |
| random      | 在所有共享订阅者中随机     |
| round_robin | 按订阅顺序           |
| sticky      | 使用上次派发的订阅者      |
| hash        | 根据发送者的 ClientId |

提示

当所有的订阅者都不在线时，仍会挑选一个订阅者，并存至其 Session 的消息队列中
