# 现代客户端配置：Clash Verge Rev & Sing-box (TUN模式)

> !> 提示：早期基于浏览器的插件代理（如 SwitchyOmega）或传统的系统代理只对“守规矩”的浏览器有效，终端、Git、Docker、后台进程经常出现“网页能开，命令超时”的割裂感。<br>
> 现代客户端已全面普及 **TUN 虚拟网卡模式**，实现全系统无死角透明接管。

---

## 一、现代核心客户端选型推荐

在原版 Clash Premium 和 Clash for Windows 停更之后，开源社区全面转向以 **Mihomo (Clash Meta)** 和 **Sing-box** 为核心的新一代客户端：

| 客户端名称 | 支持平台 | 底层核心 | 核心亮点 | 下载地址 |
| :--- | :--- | :--- | :--- | :--- |
| **Clash Verge Rev** | macOS / Win / Linux | Mihomo | 界面极简现代化、完美兼容 Clash 规则、一键开启 TUN 模式 | [GitHub Releases](https://github.com/clash-verge-rev/clash-verge-rev/releases) |
| **Sing-box GUI** | macOS / Win / Linux / Android / iOS | Sing-box | 原生支持全部现代前沿协议、内存开销极低、轻量迅速 | [Sing-box 官网](https://sing-box.sagernet.org/) |
| **Shadowrocket (小火箭)** | iOS / iPadOS / macOS | 独立核心 | iOS 端装机必备，全面支持 Reality / Hysteria 2 / 订阅 | 美区 App Store 购买 |
| **v2rayNG / NekoBox** | Android | Xray / Sing-box | 安卓端经典与现代首选 | Google Play / GitHub |

---

## 二、Clash Verge Rev 快速上手与 TUN 配置

**Clash Verge Rev** 是目前桌面端（Mac 与 Windows）综合体验最好的开源客户端。

### 1. 导入配置 / 订阅
1. 打开客户端，点击左侧 **「订阅 (Profiles)」**；
2. 在右上角地址栏粘贴你的订阅链接（支持 Clash 或 V2Ray/SS 转换格式），点击 **「导入 (Import)」**；
3. 右键激活刚导入的配置卡片。

### 2. 开启核心功能：TUN 模式（网卡接管）
为了避免命令行、Git 或其他软件不走代理，**强烈推荐开启 TUN 模式**：

1. 点击左侧 **「设置 (Settings)」**；
2. 找到 **「TUN 模式 (TUN Mode)」** 并将其开关打开；
   * *初次开启时，Windows 会提示安装 Wintun 驱动，macOS 会弹出系统权限提示，输入电脑密码允许即可*；
3. 确保 **内核堆栈 (Stack)** 默认选择 `gvisor` 或 `system`。

开启 TUN 模式后，系统代理即可关闭，你的终端（Terminal / iTerm2）、`git clone`、`curl` 以及所有应用程序都会自动受到规则接管，无需再手动 `export http_proxy`！

### 3. 分流策略选择
点击左侧 **「代理 (Proxies)」**：
* **Rule (规则模式 - 推荐)**：国内流量直连，海外受限流量自动走节点；
* **Global (全局模式)**：全盘走节点（测试连接或特殊排错时使用）；
* **Direct (直连模式)**：相当于完全关闭代理。

---

## 三、单节点直接导入示例：VLESS-Reality

如果你是自己购买 VPS 搭建的 Reality 节点，服务端通常会生成一段标准分享链接，格式如下：

```text
vless://<UUID>@<你的VPS_IP>:443?encryption=none&flow=xtls-rprx-vision&security=reality&sni=gateway.icloud.com&fp=chrome&pbk=<PublicKey>&sid=<ShortId>&type=tcp#My-Reality-Node
```

### 1. 导入方法：
* **手机端（Shadowrocket / v2rayNG）**：直接复制整行链接，打开 App 即可自动提示“检测到剪贴板节点，是否导入”，点击确认即可直接测速使用。
* **桌面端（Clash Verge Rev）**：
  * 在当前配置中，可以通过在配置的 `proxies` 字段下添加该节点：
  ```yaml
  proxies:
    - name: "My-Reality-Node"
      type: vless
      server: 你的VPS_IP
      port: 443
      uuid: 你的UUID
      network: tcp
      tls: true
      udp: true
      flow: xtls-rprx-vision
      servername: gateway.icloud.com
      reality-opts:
        public-key: 你的PublicKey
        short-id: 你的ShortId
      client-fingerprint: chrome
  ```

---

## 四、验证网络与排障小插曲

配置完成后，按以下顺序自检连接：

1. **浏览器测试**：打开浏览器无痕窗口，访问 `https://www.google.com` 确认能否秒开；
2. **终端透明代理验证**：
   打开系统终端，执行：
   ```bash
   curl -i https://ipinfo.io
   ```
   如果返回的 IP 显示为你海外 VPS 的机房 IP（如 US/JP），且国家代码正确，说明 **TUN 模式已全局无死角接管系统网络**！
3. **连不上排错三板斧**：
   * 检查电脑时间是否与北京时间一致（时间误差超过 60 秒会导致 TLS 握手鉴权被服务端拒绝）；
   * 检查 VPS 后台安全组是否放行了对应端口；
   * 客户端日志窗口（Logs）查看具体是 `connection reset` 还是 `handshake error`。
