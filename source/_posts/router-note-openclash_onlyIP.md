---
title: 记一次Openclash后台仅显示IP的问题
date: 2024-12-22 15:07:00
updated: 2024-12-22 15:07:00
categories: Router
tags:
    - 技术
    - 软路由
    - Openclash
    - Openwrt
---

## 问题描述
前段时间乱玩的时候一不小心把软路由的配置文件给整废了，好不容易救了回来却发现Openclash的后台不显示域名，只显示IP了，导致我的分流规则基本全废了。由于当时忘记截图了，图片来自网络。
![图源Github: https://github.com/vernesong/OpenClash/issues/3172#issuecomment-1500396270](/images/router-note-openclash_onlyIP/1.webp)  
本以为是转发出了问题，结果去到 DHCP/DNS 页面一看发现没有配置。
![2.webp](/images/router-note-openclash_onlyIP/2.webp)

## 环境
Openwrt版本：ImmortalWrt 18.06-k5.4-SNAPSHOT r12339-8b50c1df21 (2023-05-01) ~~（是的，一个旧到不行的版本）~~
Openclash版本：v0.46.050-beta
Meta内核版本：v1.18.0

## 解决方法
- 先说DHCP/DNS页面下显示没有“尚无任何配置”的解决方法。  
通过询问GPT发现，是Dnsmasq配置在UCI系统中不存在，即运行 `uci show dhcp` 时没有输出。这可以从rom中复制一份正确的配置文件之后重启来解决。  
```bash
cp /rom/etc/config/dhcp /etc/config/dhcp
/etc/init.d/dnsmasq restart
```
- 接着是后台仅显示IP的问题，参考Github上
[#3171 (comment)](https://github.com/vernesong/OpenClash/issues/3171#issuecomment-1506556071 "#3171 (comment)")的解决方案，在 Openclash->覆写设置->Meta设置 中将三个启用探测全部勾选，接着清理Fake-IP缓存，然后保存并应用配置即可，如图。
![3.webp](/images/router-note-openclash_onlyIP/3.webp)

## 其他配置
### Openclash 配置
运行模式：Fake-IP TUN-混合模式
本地 DNS 劫持：使用防火墙转发
自定义上游 DNS 服务器：勾选
### DHCP/DNS 配置
DNS转发：127.0.0.1#7874 (Openclash默认监听的DNS端口)

## 后记
如果想要稳定的话，千万不要随意折腾Openwrt，很多时候想着提网速或者更新什么的，要么效果不明显，要么就是像我一样玩坏了。配置这块儿写的不是很详细，如果想要更详细的或者想了解其他玩法，建议出门上恩山论坛搜，我这方面也还在自己瞎摸索中。