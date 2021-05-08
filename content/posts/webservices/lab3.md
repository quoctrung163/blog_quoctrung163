+++
title = "Lab 03 - Web service"
date = "2021-3-21"
author = "quoctrung163"
authorTwitter = "quoctrung163" #do not include @
cover = ""
tags = ["web services", "web"]
keywords = ["", ""]
description = ""
showFullContent = false
+++

### Lab 3. Sử dụng tab Network trong Developer tools của trình duyệt để phân tích chi tiết các thành phần (request line, request headers, request message body (nếu có)) của một HTTP request khi truy cập vào trang https://www.thegioididong.com/

- Request line:
+ phương thức: get 
+ url: https://www.thegioididong.com/
+ statuscode: 304 là chuyển hướng trang, không thay đổi được


- Request header:
+ accept: (kiểu nội dung chấp nhận trong trang web)
```
text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,/;q=0.8,application/signed-exchange;v=b3;q=0.9
```
+ cookie: thông tin cookie của user
```
_ga=GA1.2.1139846464.1612400297; _showchooseprovince=0; chat.username=20210104SaHeblgqA69D5J-8AGNs; chat.chating=; chat.notifychatmsg=; ins-storage-version=2; cartnew=1; TGDD_WEB_LAYOUT_DETAIL=; _gid=GA1.2.1668210160.1615793443; _hjTLDTest=1; _hjid=09be961e-aeb0-4430-b566-5973d6ce9bbd; _hjFirstSeen=1; _gat=1; _hjAbsoluteSessionInProgress=1; SvID=w10|YE8NL|YE8NI
```
+ user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)

+ do là phương thức get nên không có body
<!-- {{< linebreak >}} -->