---
title: 使用越狱iPhone部署ddns服务
date: 2024-04-15 20:21:42
tags: [iPhone,DDNS]
categories:
- [技术兴趣,设备及使用]
---
# iPhone 越狱并获取root权限
此处略，可以使用爱思助手 8.0 通过 Chimera 进行的越狱，此越狱方式并不完美，每次iPhone重启后所有越狱安装的app都会闪退，需要重新越狱一次才行。
# 在 CloudFlare 上建立一个二级域名
首先，在 Cloudflare 上申请一个域名或者在其他网站申请托管到 Cloudflare 上，并建立用来作为 DDNS 用的 A 记录和 AAAA 记录。假设是:
A 记录:     ddns.example.com      1.1.1.1
AAAA 记录   ddns.example.com      2606:4700:4700::1111
IP 地址先可以随便填。
<!--more-->
# 取得所需的 ZONE_ID、API_Token 和 RECORD_ID
ZONE_ID 就在域名概述页的右下侧，名字为区域 ID。
![20240415210230](https://raw.githubusercontent.com/shenguosai/MyPic/img/img/20240415210230.png)
然后在同一页面的下方点击获取 API 令牌。
点击创建令牌，使用“编辑区域 DNS”的模板，选择相应的域名，点击显示摘要，并创建令牌。
把生成的 API_Token 保存好。
接着打开一个 Linux 终端，执行以下代码:
```bash
ZONE_ID="你的 Zone ID"
API_TOKEN="你的API Token"
curl -X GET "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?name=ddns.example.com" \
-H "Authorization: Bearer $API_TOKEN" \
-H "Content-Type: application/json"
```
注意替换上述代码中的 ZONE_ID，API_Token，二级域名。
在返回的结果中，找到“name”与相应二级域名相符的 RECORD_ID。
# 建立 Shell 脚本
在你想运行的位置比如`root`目录建立一个脚本文件:
```bash
mkdir cfddns && cd cfddns
touch ddns.sh
```
然后用编辑器打开:
```bash
vim ddns.sh
```
复制一下代码并保存退出:
```bash
#!/bin/sh
NEW_IP=`curl -s http://ipv4.ddnspod.com`
CURRENT_IP=`cat $(dirname "$0")/current_ip.txt`
CURRENT_TIME=$(date +"%F %T")
DDNS="home.starnest.eu.org"
ZONE_ID="1234567890abcdef1234567890abcdef"
API_TOKEN="1234567890abcdef1234567890abcdef"
RECORD_ID="1234567890abcdef1234567890abcdef"
if [ "$NEW_IP" = "$CURRENT_IP" ]
then
        echo "[$CURRENT_TIME] No Change in IP Adddress" >> $(dirname "$0")/crontab_log.txt
else
curl -k -X PUT "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$RECORD_ID" \
     -H "Authorization: Bearer $API_TOKEN" \
     -H "Content-Type: application/json" \
     --data '{"type":"A","name":"'$DDNS'","content":"'$NEW_IP'","ttl":1,"proxied":false}' > /dev/null
echo $NEW_IP > $(dirname "$0")/current_ip.txt
echo "[$CURRENT_TIME] IP changed to $NEW_IP" >> $(dirname "$0")/crontab_log.txt
fi
```
若想同时适用对 ipv6 有效则使用下面的脚本(未测试):
```bash
#!/bin/sh
# 简单的使用Clodflare API来实现DDNS的脚本
NEW_IPv4=$(curl -s http://ipv4.icanhazip.com)
NEW_IPv6=$(curl -s http://ipv6.icanhazip.com)
CURRENT_IPv4=$(cat $(dirname "$0")/current_ipv4.txt)
CURRENT_IPv6=$(cat $(dirname "$0")/current_ipv6.txt)
CURRENT_TIME=$(date +"%F %T")
# 填入DDNS域名
DDNS="ddns.example.com"
# ZONE_ID在Clouflare域名页面上获取
ZONE_ID="1234567890abcdef1234567890abcdef"
# API_TOKEN在Clouflare个人资料页面上创建
API_TOKEN="1234567890abcdef1234567890abcdef"
# RECORD_ID获取应执行以下代码：
# ZONE_ID="你的 Zone ID"
# API_TOKEN="你的API Token"
# curl -X GET "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records?name=example.com" \
#     -H "Authorization: Bearer $API_TOKEN" \
#     -H "Content-Type: application/json"
RECORD_ID="1234567890abcdef1234567890abcdef"
if [ -z "$NEW_IPv4" ] && [ -z "$NEW_IPv6" ]; then
  echo "[$CURRENT_TIME] Failed to get IP address" >> $(dirname "$0")/crontab_log.txt
  exit 1
elif [ "$NEW_IPv4" = "$CURRENT_IPv4" ] && [ "$NEW_IPv6" = "$CURRENT_IPv6" ]; then
  echo "[$CURRENT_TIME] No change in IP address" >> $(dirname "$0")/crontab_log.txt
else
  if [ -n "$NEW_IPv4" ] && [ "$NEW_IPv4" != "$CURRENT_IPv4" ]; then
    curl -k -X PUT "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$RECORD_ID" \
      -H "Authorization: Bearer $API_TOKEN" \
      -H "Content-Type: application/json" \
      --data '{"type":"A","name":"'$DDNS'","content":"'$NEW_IPv4'","ttl":1,"proxied":false}' > /dev/null
    echo "$NEW_IPv4" > $(dirname "$0")/current_ipv4.txt
    echo "[$CURRENT_TIME] IPv4 address changed to $NEW_IPv4" >> $(dirname "$0")/crontab_log.txt
  fi
  if [ -n "$NEW_IPv6" ] && [ "$NEW_IPv6" != "$CURRENT_IPv6" ]; then
    curl -k -X PUT "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records/$RECORD_ID" \
      -H "Authorization: Bearer $API_TOKEN" \
      -H "Content-Type: application/json" \
      --data '{"type":"AAAA","name":"'$DDNS'","content":"'$NEW_IPv6'","ttl":1,"proxied":false}' > /dev/null
    echo "$NEW_IPv6" > $(dirname "$0")/current_ipv6.txt
    echo "[$CURRENT_TIME] IPv6 address changed to $NEW_IPv6" >> $(dirname "$0")/crontab_log.txt
  fi
fi
```
# 执行 Shell 脚本并设置定时任务
## 手动执行确认
先手动执行一次脚本，然后检查 Cloudflare 解析记录和同文件夹下生成的 crontab_log.txt，确认 IP 是否正确更新。
```bash
/bin/sh ddns.sh
```
## 设置定时任务
若上述脚本执行无误，则使用 plist 设置定时任务。
首先，要新增被管理项需要新增一个`.plist`放入苹果的管理文件夹下，然后使其被加载后执行。苹果根据用户的角色提供了不同的 Launtch 存放位置。
```bash
~/Library/LaunchAgents          # 当前用户定义的任务
/Library/LaunchAgents           # 系统管理员定义的任务
/Library/LaunchDaemons          # 管理员定义的系统守护进程任务
/System/Library/LaunchAgents    # 苹果定义的任务
/System/Library/LaunchDaemons   # 苹果定义的系统守护进程任务
```
很显然，我们最好不要使用下面两个位置的，我这里使用的是`/Library/LaunchDaemons`。
```bash
cd /Library/LaunchDaemons
touch starnest.ddns.plist   #前面的名字随便起，最后以`.plist`结尾
```
然后编辑`starnest.ddns.plist`文件:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>starnest.ddns</string>
    <key>ProgramArguments</key>
    <array>
      <string>/bin/sh</string>
      <string>/var/root/script_service/ddns/ddns.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>StartInterval</key>
    <integer>180</integer>
</dict>
</plist>
```
> 上面的 StartInterval 中的时间不是很确定，使用`date`命令测试时 interger 设置为 1 会没 10 秒执行一次任务，但是 ddns 脚本中设置 180 应该是 30 分钟执行一次，但根据日志结果来看是 1 小时 3~4 次，时间不是太固定。但对 ddns 来说每小时 3~4 的频度足够了。
## 启动定时任务
```bash
launchctl load /Library/LaunchDaemons/starnest.ddns.plist
```
确认是否启动成功
```bash
launchctl list | grep starnest
```
```
-       0       starnest.ddns
```
第一列的 PID，第二列是 status，当 status 显示为 0 时表示任务正常进行。
我刚开始 status 显示 127，这个可以用以下命令来初步确认问题。
```bash
launchctl error 127
```
```
127: The specified service did not ship with the operating system
```
最后发现是`plist`文件中脚本路径发生的错误。
