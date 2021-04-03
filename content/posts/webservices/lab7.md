+++
title = "Lab 7 - Web service"
date = "2021-3-27"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["web services", "web"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

- Ví dụ lỗi:
```xml
<rss xmlns:slash="http://purl.org/rss/1.0/modules/slash/" version="2.0">
    <channel>
        <title>Tin mới nhất - VnExpress RSS</title>
        <description>VnExpress RSS</description>
    </channel2>
</rss>
```
* {{< figure src="/img/web_services/2.png" title="lab1_1" >}}
- Sữa lỗi
```xml
<rss xmlns:slash="http://purl.org/rss/1.0/modules/slash/" version="2.0">
    <channel>
        <title>Tin mới nhất - VnExpress RSS</title>
        <description>VnExpress RSS</description>
    </channel>
</rss>
```
* {{< figure src="/img/web_services/3.png" title="lab1_1" >}}