---
layout: post
title: GitHub page with custom domain + Cloudflare
---

想學的東西很多，聽過看過很容易忘記，於是開始寫 blog 梳理思緒，留下文字紀錄也比較不會忘記，此篇記錄如何在 GitHub page 套用 custom domain 並使用 Cloudflare 管理 domain。

先準備好下列：
1. GitHub page （[使用 GitHub 免費製作個人網站](https://gitbook.tw/chapters/github/using-github-pages.html)）
2. 購買 domain （[Gandi.net](https://www.gandi.net/zh-Hant)）


### 在建立好的 GitHub page 設定 custom domain

進入 GitHub page 的 repo setting
，設定 Custom domain，按下 save，就會在此 repo 產生 CNAME ，如此一來 GitHub 就會知道要導向哪個 domain。

<img src="https://i.imgur.com/tXDCgjb.png">

### 註冊 Cloudflare
註冊完後，在 Cloudflare 新增網站，接著可選擇免費方案。

在DNS/Cloudflare找到兩個名稱伺服器(Namaservers)。

回到 Gandi.net (域名、名稱伺服器)設定外部名稱伺服器，將 Cloudflare Nameservers 設定於此。

### 設定 Cloudflare DNS

刪除 Type 為 A 的設定，將 GitHub 方官提供的 4 組 ip 設定為 Type A，名稱就是 custom domain 。

<img src="https://i.imgur.com/ofnv0h3.png">

More details: [Managing a custom domain for your GitHub Pages site](https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site)

### Cloudflare page rule

在網頁規則 (page rule)新增設定“一律使用HTTPs”，接者等待SSL certificate。

<img src="https://i.imgur.com/cBU5JuY.png">

等待轉網址成功後，發現 css 沒成功套上， console 跳出錯誤訊息`This request has been blocked; the content must be served over HTTPS`。

解決方案：
1. [Web API error](https://stackoverflow.com/questions/52130918/web-api-error-this-request-has-been-blocked-the-content-must-be-served-over-h/52133425)
在HTML <head></head> 新增下列： `<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">`。

2. [Securing your GitHub Pages site with HTTPS](https://help.github.com/en/github/working-with-github-pages/securing-your-github-pages-site-with-https)
把 HTTP 改成 HTTPS。


參考：
1. [如何將 GitHub Pages 套上個人網域及 Cloudflare SSL](https://blog.moli.rocks/2018/07/11/how-to-set-custom-domain-and-cloudflare-ssl-to-github-pages/)
2. [Github Pages 自訂網域免費升級 https](https://wcc723.github.io/design/2018/07/27/gh-pages-https/)