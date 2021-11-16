---
title: 记录关于shadowsockts工具的应用
date: 2021-11-16 19:10:55
tags: Linux,ss
category: 服务器运维
---

> 场景描述：公司部分 ip 被阿里限制访问，找 it 支撑部门沟通，回复已经与阿里方沟通但迟迟未得到解决。阿里云、egg、element 文档、高德地图脚本等均无法正常访问，已经严重影响日常开发工作，并且公司网络访问 github 一直不稳定，故考虑用手边闲置的 1c2g 服务器搭建 shadowsockets 工具，代理访问阿里系网站及 github。

#### 记录实现步骤如下

1. 在服务器上下载 Python-pip 工具

```sh
yum install python-pip
```

2. 利用工具下载 shadowsockts

```
pip install shadowsockts
```

3. 编辑 shadowsockts 配置文件

```
vim /etc/shadowsockts.json
```

password 为配置 port-password 键值对，代表对应端口的密码

```json
{
  "server": "0.0.0.0",
  "local_address": "127.0.0.1",
  "local_port": 1080,
  "password": {
    "xxxx", "xxxxx"
  },
  "timeout": 300,
  "method": "aes-256-cfb",
  "fast_open": false
}
```

4. 在阿里云上配置实例安全组规则

5. 启动服务

```
ssserver -c /etc/shadowsockts.json -d start
```

6. 查看日志

```
tail -f -n 100 /vat/log/shadowsockts.log
```

7. 本地终端使用小飞机客户端连接代理服务器，配置用户 pac 规则，开启 pac 代理模式即可。每次更新用户规则后需要禁启用连接后配置生效。[pac 规则配置语法说明](https://adblockplus.org/en/filter-cheatsheet)

### 闲置服务器仅 1M 带宽，但是已经满足日常开发需求，访问 github 也流畅了很多，问题得到完美解决。
