# SS/SSR

> [!WARNING]
> **【历史技术归档 · 已淘汰】**
> * **技术现状**：原版 Shadowsocks (SS) 及 SSR 对称加密协议在当下抗审查环境中已基本失效，新自建服务器裸跑通常在几小时至数天内即遭封禁。
> * **为何淘汰与失效（技术根因）**：
>   1. **高熵特征识别**：正常互联网流量包含大量结构化或半明文报文头，而早期 SS 流量是全密文输出，信息熵（Entropy）极其接近 1.0。GFW 部署的机器学习分类器可根据数据流的字节熵分布轻而易举将其识别为非正常流量。
>   2. **主动探测（Active Probing）**：GFW 捕获可疑连接后，会向目标服务器伪造发送特定畸变重放包。早期 SS 实现缺乏健壮的重放防御与防探测机制，一旦回包即坐实代理身份并立即拉黑 IP/端口。
> * **权威研究与文献链接**：
>   * 顶级安全学术顶会 USENIX Security 论文：[How China Detects and Blocks Shadowsocks (USENIX Security '20)](https://gfw.report/publications/usenixsecurity20/zh/)
>   * GFW-Report 深度技术复盘：[深入分析中国防火长城对 Shadowsocks 的检测与阻断机制](https://gfw.report/blog/gfw_shadowsocks/)
> * **现代替代方案与参考**：
>   * 推荐直接阅读本书新章节：[现代协议演进与抗审查技术](/modern/evolution) 以及 [VLESS 与 XTLS-Reality](/modern/reality)
>   * 现代核心工具官网：[Sing-box 官方文档](https://sing-box.sagernet.org/) ｜ [Project X / Xray 官方文档](https://xtls.github.io/)

!>  简单来说代理与VPN的最大区别就在于代理不会虚拟一块独立的网卡<br>
  多数免费代理与VPN是严格[NAT类型](/abc/4nat.md)，以限制游戏与下载等操作<br>
 ssr可以使用ss链接与二维码，反之是不行的，且ss只能连接兼容ss的服务器<br>
 若想较为深入的了解ss链接的含义，[请参考ss、ssr、v2ray链接解析章节](/append/srvurl.md)<br>
 ss与ssr都做了socks代理端口对http协议的兼容，所以并不需要额外的代理转发<br>
 全局模式即被的代理软件所有网络均走代理路线，直连模式即正常访问不走任何代理<br><br>


## SS

?> 在直接关机而没先将代理关闭的情况下，计算机再次启动时，设置在系统中代理配置还是生效的，这也就是用过代理软件后，却无法上网的原因

### 入门

访问[ss站点](https://free-ss.tk/)，右键扫描二维码

<!-- ![](https://ipfs.io/ipfs/QmWS9eJJi7dnMXjG7jxYAz7NDDCLHnrtSfc6viNRjbBjc9?2.png) -->

![](https://i.postimg.cc/C1v3LX5P/2018-04-30-105508.png)

 此时已经可以连接互联网了，如果你的系统不是自动设置的，请看<a href="#/proxy/ss-ssr?id=配置">配置</a>

右键允许来自局域网的连接

<!-- ![](https://ipfs.io/ipfs/QmbNUAL9vmXcnAkWP15XxevvLqpED2tbAxxnVCeGDs3o9X?1.png) -->

![](https://i.postimg.cc/J0fdsLLq/2018-05-05-032022.png)

`网络和共享中心`-&gt;`wlan`-&gt;`详细信息`查看本机网卡IP地址

<!-- ![](https://ipfs.io/ipfs/QmdwEi4zS8DNWx8gzkykPAoBkocQguEEP4QYhZFQV8Kwj9?4.png) -->

![](https://i.postimg.cc/Vvsxz4Ds/2018-05-05-032400.png)

`高级`设置`代理`选择`手动`，按照如下信息设置

<!-- ![](https://ipfs.io/ipfs/QmfU5EVwSUgyNtKFbetxfR1pvcyQTgbmM1y5Rp7QYkuX1b?1.png) -->

![](https://i.postimg.cc/mkCKNDVq/x1.png)

 一些朋友可能对连接互联网的网速要求较高，也可使用[speedtest](http://www.speedtest.net/)进行测试

<!-- ![](https://ipfs.io/ipfs/QmRfQ2LhCek5jw7UDBxwC2Y9Qm8VLjP17Ehhgh99Kw7Uod?3.png) -->

![](https://i.postimg.cc/zXw9yyyH/x2.png)

### 配置

有些系统如win7，连接服务器后还需在internet属性中手动设置本机地址与sock5代理端口

<!-- ![](https://ipfs.io/ipfs/QmQBdt4QM9GKcgfFdXh1LtKh45ubyrTqhjEgVHUBk9VfG4?4.png) -->

![](https://i.postimg.cc/k4tHqGr4/2018-04-28-224352.png)

PAC模式即脚本配置模式，收录的网址走代理路线，没有收录的地址则不走代理路线即正常访问。例如将Google加入代理访问列表，配置规则如下：

```text
 ".google.com",
"||google.com",
```

在`pac`选项中-&gt;`编辑本地pac文件`即可

<!-- ![](https://ipfs.io/ipfs/QmeHE8dTsEEQhvQRWBjwzKeioyprepRha6vFFYpce4i22o?1.png) -->

![](https://i.postimg.cc/xTP40BZK/2018-04-28-230423.png)

### SS分享

右键-&gt;`服务器`-&gt;`分享服务器配置`，如图

![](https://i.postimg.cc/bv46CPGj/2018-53px8.png)

随后可以看到相关ss链接与二维码生成

![](https://i.postimg.cc/90hL2Bt8/2018-06-09-174922.png)

可将二维码截图或是复制ss链接分享给他人，扫码或粘贴导入都行

![](https://i.postimg.cc/FFyPZ2N2/2018-06-09-181034.png)

## SSR

### 订阅功能

右键->`服务器订阅`->`SSR服务器订阅设置`
<!-- ![](https://ipfs.io/ipfs/QmX4z2VDbj5EDvzRzBHTiyqYsTvvxbgDi3pFwhiLfLLNFL?1.png) -->

![](https://i.postimg.cc/wvKV5kPq/2018-04-28-235146.png)

点击add添加按钮，并导入此条订阅：https://prom-php.herokuapp.com/cloudfra_ssr.txt
<!-- ![](https://ipfs.io/ipfs/QmNbaKnwt9E447xLzndAZvCHDByMbA6rZn4AsdDbeuFDuP?2.png) -->

![](https://i.postimg.cc/YChfJVB2/2018-06-09-215048.png)

<!-- ![](https://ipfs.io/ipfs/QmfXCT9yWSxPq4G7QuU9b1RzmFWZodAkY2Pzrt7iGHko5X?1.png) -->

再右键->`服务器订阅`->`更新ssr服务器订阅（不通过代理）`

![](https://i.postimg.cc/jSpNBShv/2018-04-28-235337.png)

订阅成功后会有如下提示
<!-- ![](https://ipfs.io/ipfs/QmdteWfXcW3NzJrB8gbxmF83eoybYfBoLThFEC6f8CwYCw?1.png) -->

![](https://i.postimg.cc/SQ1nwKS8/2018-04-28-235358.png)

再打开SSR可看到导入了多条账号信息

![](https://i.postimg.cc/TPcwQdNK/2018-06-09-220222.png)


订阅的好处：

* 一键导入多条服务器地址
* 不需要经常打开站点网址即可更新服务器配置（取决于订阅源）

### SSR分享

打开ssr选中ssr链接即可

![](https://i.postimg.cc/SNzx37tF/2018-06-09-190728.png)



<!-- !> ssr可以使用ss链接与二维码，反之是不行的，且ss只能连接兼容ss的服务器<br>
 若想较为深入的了解ss链接的含义，[请参考ss、ssr、v2ray链接解析章节](/append/srvurl.md)<br>
 ss与ssr都做了socks代理端口对http协议的兼容，所以并不需要额外的代理转发<br>
 全局模式即被的代理软件所有网络均走代理路线，直连模式即正常访问不走任何代理 -->
<!-- ps:

* ssr可以使用ss链接与二维码，反之是不行的，且ss只能连接兼容ss的服务器
* ss与ssr都做了socks代理端口对http协议的兼容，所以并不需要额外的代理转发
* 全局模式即被的代理软件所有网络均走代理路线，直连模式即正常访问不走任何代理 -->
