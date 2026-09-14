# Betternet

> [!WARNING]
> **【历史技术归档 · 安全警示与彻底淘汰】**
> * **现状与警示**：Betternet 等所谓“免费一键式商业 VPN”存在极其严重的安全与隐私隐患，新读者切勿下载或使用。
> * **为何淘汰与危险（行业黑幕）**：
>   1. **商业模式陷阱与恶意代码**：澳大利亚联邦科学与工业研究组织（CSIRO）与加利福尼亚大学伯克利分校的研究表明，Betternet 在其移动客户端中嵌入了高达 14 个第三方广告与追踪库，并含有针对用户的恶意重定向代码。
>   2. **无日志承诺破产与隐私倒卖**：绝大多数宣称免费的商业 VPN 依靠收集并出售用户明文浏览记录、真实 IP 和设备指纹给广告中介与数据经纪人来盈利。
>   3. **弱加密与易被阻断**：其底层多使用极容易被 GFW 识别的古老 OpenVPN 或自定义未混淆协议，在国内网络中握手几乎完全失败。
> * **学术界安全审计报告链接**：
>   * CSIRO 与 UC Berkeley 权威论文：[An Analysis of the Privacy and Security Risks of Android VPN Permission-enabled Apps](https://research.csiro.au/ng/wp-content/uploads/sites/106/2016/08/vpn-study.pdf)
>   * 安全媒体披露：[Wired - Free VPNs Are Selling Your Data](https://www.wired.com/story/free-vpn-data-privacy-risks/)
> * **现代正规替代方案**：
>   * 个人自建或合规分流网络方案，请直接阅读：[代际演进与对抗全景](/modern/evolution)

!> 再次说明：<br>
简单来说VPN与代理的最大区别就在于代理不会虚拟一块独立的网卡<br>
多数的免费VPN与代理有着严格的[NAT类型](4nat.md)，以限制游戏与下载等相关操作

点击 `connect` 等一会 PC 即可连接上互联网

<!-- ![](https://ipfs.io/ipfs/QmWFRGy8fQr35qK5RujpWdnHyQjMWvjESRxnQK84uQrhcw?3.png) -->

![](https://i.postimg.cc/x18kPk9D/2018-04-29-022009.png)


打开`移动热点`，创建成功后，并右键选择`设置`

<!-- ![](https://ipfs.io/ipfs/QmfPtCEk3dqjjXeXHW67paE6TuRRm8t144VpweAJzU5Ux5?3.png) -->

![](https://i.postimg.cc/hPbw0jpr/2018-05-08-213716.png)

可设定WiFi热点名称与密码，若系统是win7，[就请看这里](/append/win7-wifi)

<!-- ![](https://ipfs.io/ipfs/Qmb5xZZWGN73dWHXHHfTPTxSPmqZQEsPRkaGwhGgHYG1SS?1.png) -->

![](https://i.postimg.cc/L6RQhS2Z/2018-05-08-214959.png)

`控制面板`->`网络共享中心`->`更改适配器设置`找到VPN软件开启的网卡，右键`属性`

<!-- ![](https://ipfs.io/ipfs/QmRPSE29AQPX37pcKyH6HkWcg18pqAymp66J68ziFTnEie?4.png) -->

![](https://i.postimg.cc/q7tjWMXW/2018-05-08-221121.png)

在`共享`选择`允许其他网络用户通过此计算机的Internet连接来连接`，并在`家庭网络连接`选择热点网卡

<!-- ![](https://ipfs.io/ipfs/QmaWy3yjxn1a88qVjKrinU2wYEyYLpjGausVvG1UdoNSqu?0.png) -->

![](https://i.postimg.cc/B6TcG887/2018-05-08-221920.png)

打开手机`WiFi`设置

<!-- ![](https://ipfs.io/ipfs/QmdUMKKiFa1Fj7wottZXy8zY7wq7m78TudGDGkDinAX1SZ?2.png) -->

![](https://i.postimg.cc/dVg8kRrS/QQ20180508224410.png)

测试

<!-- ![](https://ipfs.io/ipfs/QmfCDDEGWhFb2nD7LrLu2gmVWdhVzBgDc59qDnmGMfaYJE?1.png) -->

![](https://i.postimg.cc/nrV7b3fM/QQ20180508224420.png)