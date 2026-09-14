# goagent

> [!WARNING]
> **【史前考古 · 方案已永久失效】**
> * **技术现状**：GoAgent 是 2010~2014 年间基于 Google App Engine (GAE) 免费云主机的划时代免费工具，现已永久失效，代码库亦已清空封存。
> * **为何失效（历史原因）**：
>   1. **Google 服务在华 IP 被全面封死**：2014 年 5 月底起，GFW 对 Google 全球数以万计的 IP 地址（包括 GAE 数据中心 IP）进行了系统性的 IP 路由丢包和 SSL 握手阻断，导致客户端无法直接连接到 GAE 平台。
>   2. **根证书伪造安全隐患**：GoAgent 依赖客户端本地导入虚假的根证书以解密 HTTPS 流量，这一中间人（MITM）机制存在极大的本地安全隐患，现代浏览器（Chrome/Firefox）已通过 HSTS、证书透明度（CT）和严格安全策略彻底封禁此行为。
>   3. **GAE 架构演进**：Google App Engine 在升级至新一代运行环境后，全面限制了底层 Raw Socket 的调用行为，原有的转发代理脚本已无法运行。
> * **历史参考链接**：
>   * 维基百科：[GoAgent 历史条目](https://zh.wikipedia.org/wiki/GoAgent)
>   * 时代技术回顾：[月光博客 - Google App Engine 代理的发展与终结](https://www.williamlong.info/archives/3874.html)

!> goagent已停止维护并清空了相关仓库，基于此的相关衍生项目xx-net自身问题颇多，且含有服务质量低下的付费功能，并不推荐使用

GoAgent通过使用GAE的服务器作为中转绕过了GFW。它的运作流程是浏览器代理设置将请求的数据重定向到client，对数据加密后并发送到GAE的server，再将数据解密并请求需要的数据回传给client。

![](https://i.postimg.cc/nrqdmmrj/goagengstuture.jpg)

