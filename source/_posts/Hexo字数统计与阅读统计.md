---
title: Hexo字数统计与阅读统计
date: 2023-11-20 20:54:12
tag: Hexo
categories: 
- [技术兴趣,网络服务]
---

1. 安装```hexo-wordcount```插件
   ```npm install hexo-wordcount --save```
2. 安装```hexo-symbols-count-time```插件
   ```npm install hexo-symbols-count-time --save```
3. 安装```eslint```
   ```npm install eslint --save```
<!--more-->
4. 在hexo配置文件中添加```symbols_count_time```配置语句
   ```
   symbols_count_time:
    symbols: true               # 文字字数统计
    time: true                  # 文章阅读时长
    total_symbols: true         # 站点总字数统计
    total_time: true            # 站点总阅读时长
    exclude_codeblock: false    # 排除代码字数统计
   ```
5. 在主题配置文件中添加```symbols_count_time```配置语句
   ```
   symbols_count_time:
    separated_meta: true        # 是否另起一行(true的话不和发表时间等同一行)
    item_text_post: true        # 首页文章统计数量前是否显示文字描述(本文字数、阅读时长)
    item_text_total: false      # 页面底部统计数量前是否显示文字描述(站点总字数、站点阅读时长)
    awl: 4                      # Average Word Lenth
    wpm: 275                    # Words Per Minute（每分钟阅读词数
    suffix: mins.
   ```
6. 在主题配置文件中配置wordcount
   ```
   post_wordcount:          # 字数统计
    item_text: true         # 是否显示文字
    wordcount: true         # 显示字数
    min2read: true          # 显示阅读时间
    totalcount: true        # 显示总数
    seperated_meta: true    # 是否分开
   ```

设置完成！