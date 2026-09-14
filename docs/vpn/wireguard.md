# wireguard

> [!WARNING]
> **【历史技术归档 · 原生协议已被封锁】**
> * **技术现状**：裸跑原生 WireGuard 在国内跨境网络下已基本不可用，端口会被运营商瞬间丢包。
> * **失效原因**：WireGuard 握手包头结构和初始握手包长度（148 字节）是全公开的标准特征，在没有任何混淆的情况下，三大运营商骨干网的 DPI 会将其识别为非法隧道并实施黑洞丢包。
> * **权威文献与替代**：
>   * 详细技术分析见：[How China Blocks WireGuard (GFW Report)](https://gfw.report/blog/gfw_wireguard/)
>   * 如果需要利用虚拟网卡全盘接管流量，推荐使用现代客户端的 TUN 模式：[现代内核底座与 TUN 模式](/abc/core-and-tun) 与 [现代客户端配置](/proxy/modern-clients)；若需要保护 WireGuard 流量，可关注开源混淆衍生版本 [AmneziaWG](https://amnezia.org/)。

进入[wireguard](https://www.wireguard.com/install/)，下载

![](https://i.postimg.cc/d0Ny2kxH/Snipaste-2019-10-04-10-49-28.png)

打开wireguard选择 `add empty tunnel`并复制`public key` 公钥键值

![](https://i.postimg.cc/FR2w1Vkf/00-05.png)

进入[cryptostorm.is/wireguard](https://cryptostorm.is/wireguard)，将公钥复制到`your wireguard public key`选项，`add key`继续，并生成的配置文件

![](https://i.postimg.cc/MHB0Ky1p/27-48.png)

将配置文件复制到`public key`下方配置信息框内，并删除如下不必要的字段

```
[Interface]
PrivateKey = YOUR_PRIVATE_KEY
```

![](https://i.postimg.cc/BbRhmSBX/41-15.png)

点击`active`激活

![](https://i.postimg.cc/65gwLTRh/50-10.png)

测试

![](https://i.postimg.cc/zvz1SjPK/56-26.png)

> 参考自油管 [Siemens Tutorials](https://www.youtube.com/channel/UCmvvn2qsP77_7XUB0omMeCw/about?pbjreload=10) 关于wireguard翻墙视频