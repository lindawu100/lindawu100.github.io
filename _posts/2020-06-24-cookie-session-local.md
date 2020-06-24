---
layout: post
title: Cookie, Session storage, Local storage
---

### Session cookies

server 在使用者端的瀏覽器儲存的一小塊資料，當瀏覽器與 server 互動時，cookies 就會隨 HTTP request/response 傳遞。

特性：
1. 資料存量：4kb
2. 生命週期：手動設定
3. 是否隨著 http req/res：是

### Session storage

> Opening a page in a new tab or window creates a new session with the value of the top-level browsing context, which differs from how session cookies work.

💡 每次開啟新的分頁或視窗就會產生新的 session。

> Opening multiple tabs/windows with the same URL creates sessionStorage for each tab/window.

💡 多個分頁或視窗狀況下，即使是同一個 URL ，每個分頁或視窗會有各自的 session。

特性：
1. 資料存量：多為5Mb (依據瀏覽器而有所不同)
2. 生命週期：當分頁 (tab) 關掉時，就失效了
3. 是否隨著 http req/res：否

### Local storage

特性：
1. 資料存量：多為5Mb(依據瀏覽器而有所不同)
2. 生命週期：不會失效，除非手動刪除
3. 是否隨著 http req/res：否

💡 Local storage 與 Session storage 十分相似，不同的地方是生命週期 (lifetime) 以及可取用的範圍 (scope)。

參考：
1. [Using HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies)
2. [Web storage](https://en.wikipedia.org/wiki/Web_storage#localStorage)
3. [重要的基礎：Cookie v.s Session Storage v.s Local Storage](https://medium.com/@brianwu291/cookie-sessionstorage-localstorage-and-cookie-based-token-based-authentication-da7af80ec00d)
4. [Window.sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)