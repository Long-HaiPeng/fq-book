# 现代节点获取与自建落地指南 (Reality / Hysteria 2)

> 时代在变，猫鼠游戏的规则也在变。在 2018 年，买台小鸡装个 `shadowsocks-libev` 就能稳用一年；但在 2024~2026 年的今天，这种做法不仅容易被几小时封端口，还会让你陷入“IP 被墙 ➔ 花钱换 IP ➔ 再次被墙”的死循环。
> 
> 本篇整理当前主流、成熟且抗封锁的节点落地方法论：**借壳伪装的 VLESS-Reality** 与 **基于 QUIC 暴力抗弱网的 Hysteria 2**。

---

## 一、现代获取节点的方式

获取现代节点通常有两条路径，读者可根据自身技术背景与时间成本选择：

### 1. 订阅型（商业聚合服务 / 机场）
* **现状**：现代主流聚合服务已全面支持 Reality 与 Hysteria 2 协议，并以 Base64 或 Clash / Sing-box 订阅链接的形式分发。
* **选购注意要点**：
  * **协议支持**：优先选择明确标明提供 `Reality` 或 `Hysteria 2` 节点的提供商，尽量避免还在主推纯 SS/SSR 的服务商；
  * **节点入口**：推荐选择具有**国内 BGP / 专线（如 IPLC/IEPL）**中转的节点。专线不过公网 GFW，延迟更低且敏感时期基本不中断；
  * **退款与月付**：任何外部服务均存在不可抗力风险，**切记“月付保平安”**，切勿贪图年付折扣一次性充值大额资金。

### 2. 境外云服务器自建（掌握全部掌控权）
* **VPS 机房选型参考**：
  * 常见国际大厂：[AWS Lightsail](https://aws.amazon.com/cn/lightsail/)（东京/新加坡，月付 3.5 刀起）、[Linode / Akamai](https://www.linode.com/)、[DigitalOcean](https://www.digitalocean.com/)、[Vultr](https://www.vultr.com/)；
  * 针对中国大陆网络优化的机房：搬瓦工（BandwagonHost，含 CN2 GIA 高质量线路）、DMIT 等；
  * **IP 纯净度核验**：购买 VPS 后第一件事，使用 [IPinfo](https://ipinfo.io/) 或 `curl ipinfo.io` 检查 IP 属性是否为机房原生，并使用 `ping` 确保 IP 未在购买前就被 GFW 封锁。

---

## 二、自建方案 A：VLESS + XTLS-Reality（抗主动探测主力）

Reality 最大优势是**无需购买域名、无需配置 SSL 证书**，直接借用海外真实大厂网站的证书。

### 1. 工具与一键内核部署
推荐使用开源社区经过多年审计的通用管理脚本（基于官方 Xray-core / Sing-box 内核）：

在 VPS 终端（Ubuntu / Debian）运行：

```bash
# 安装 Xray 官方管理工具（支持一键 Reality）
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)"
```

### 2. 生成 Reality 所需密钥对与 ShortId
Reality 客户端需要使用服务端的公钥进行握手认证，先在服务器生成公私钥：

```bash
# 生成私钥和公钥
xray x25519
```
输出示例：
```text
Private key:  <这里是服务端私钥，填入服务端配置>
Public key:   <这里是客户端公钥，填入客户端配置>
```

生成随机 ShortId：
```bash
openssl rand -hex 8
```

### 3. 服务端配置精简模板 (`/usr/local/etc/xray/config.json`)

```json
{
  "inbounds": [
    {
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "id": "你的UUID-用-xray-uuid-生成",
            "flow": "xtls-rprx-vision"
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "show": false,
          "dest": "gateway.icloud.com:443",
          "xver": 0,
          "serverNames": [
            "gateway.icloud.com"
          ],
          "privateKey": "你刚才生成的PrivateKey",
          "shortIds": [
            "你刚才生成的ShortId"
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom"
    }
  ]
}
```

* **配置关键点**：
  * `port`：必须强烈推荐 `443`，完美混入常规 HTTPS；
  * `dest` 与 `serverNames`：借壳目标。推荐使用物理距离靠近该 VPS、支持 TLS 1.3 且国内可正常直连的海外大厂域名（如 `gateway.icloud.com`、`www.lovelive-anime.jp`）。

### 4. 服务端启动与检查
```bash
sudo systemctl restart xray
sudo systemctl enable xray
sudo systemctl status xray
```

---

## 三、自建方案 B：Hysteria 2（恶劣弱网与丢包提速主力）

如果你的 VPS 线路较差、晚高峰看 4K 严重卡顿，推荐搭建基于 QUIC/UDP 的 Hysteria 2。

### 1. 一键安装 Hysteria 2 官方服务
```bash
bash <(curl -fsSL https://get.hy2.sh/)
```

### 2. 极简服务端配置 (`/etc/hysteria/config.yaml`)

```yaml
listen: :443

# 自签证书或借助免费证书（Hy2 支持客户端忽略证书校验）
tls:
  cert: /etc/hysteria/server.crt
  key: /etc/hysteria/server.key

auth:
  type: password
  password: "你的自定义连接密码"

# 伪装页面（当非客户端流量访问时返回正常网页）
masquerade:
  type: proxy
  proxy:
    url: https://news.ycombinator.com/
    rewriteHost: true

# 开启端口跳变支持（对抗运营商单端口 UDP QoS）
# 需要配合 iptables 转发规则使用
```

启动命令：
```bash
sudo systemctl restart hysteria-server
sudo systemctl enable hysteria-server
```

---

## 四、自建与选型避坑总结

1. **借壳目标不要乱选**：切勿把借壳域名设为 `google.com`、`twitter.com` 或 `youtube.com` 等国内原本就已经被 DNS 阻断的大站，否则连握手的第一步都会被 GFW 拦截。
2. **防火墙安全组放行**：
   * Reality 走的是 TCP，安全组必须放行 TCP `443` 端口；
   * Hysteria 2 走的是 UDP，安全组必须放行 UDP `443` 端口。
3. **日常运维排查**：
   * 客户端连不上时，第一步先在 VPS 运行 `journalctl -u xray -n 30` 或 `journalctl -u hysteria-server -n 30` 查阅实时握手日志。
