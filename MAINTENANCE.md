# 《这本书能让你连接互联网》项目维护手册

本手册专门面向**仓库维护者与贡献者**，梳理项目的架构规范、本地调试方式、新增章节流程以及上游同步准则。

---

## 一、项目架构与工作原理

本项目采用 **[Docsify](https://docsify.js.org/)** 运行时渲染引擎，没有复杂的 Webpack/Vite 编译构建步骤，所有 Markdown 文件在浏览器端动态解析。

```
fq-book/
├── README.md              # 仓库主介绍（面向读者与 GitHub 访问者）
├── MAINTENANCE.md         # 仓库维护手册（面向维护者，即本文档）
├── deploy.sh / push.sh    # 早期辅助脚本
└── docs/                  # Docsify 核心站点目录（GitHub Pages 部署根目录）
    ├── index.html         # Docsify 入口配置、插件引入与主题定制
    ├── _sidebar.md        # 左侧目录树（必须手动维护新增章节）
    ├── custom.css         # 自定义样式微调
    ├── README.md          # 站点首页/前言
    ├── modern/            # 【2022~2025 现代对抗技术专章】
    ├── abc/               # 经典计算机网络原理与科普
    ├── proxy/             # 代理工具章节
    ├── vpn/               # VPN 隧道章节
    └── ...                # 其他功能与专题目录
```

---

## 二、本地预览与调试

因为是纯静态架构，你可以选择任意静态服务器在本地快速预览渲染效果：

### 推荐方式 1：使用 Python 内置服务器（零安装依赖）

```bash
cd docs
python3 -m http.server 3000
```
打开浏览器访问：`http://localhost:3000` 即可实时预览。

### 推荐方式 2：使用 docsify-cli（支持文件保存热重载）

```bash
# 全局安装（仅需一次）
npm install -g docsify-cli

# 在仓库根目录启动本地服务
docsify serve docs
```
打开浏览器访问：`http://localhost:3000`。每次保存 Markdown 文件，浏览器将自动热刷新。

---

## 三、如何新增或修改章节？

### 步骤 1：编写 Markdown 文档
* 存放位置规范：
  * 现代协议与抗审查新特性 ➔ 存入 `docs/modern/`
  * 计算机网络与安全底层科普 ➔ 存入 `docs/abc/`
  * 实操客户端与工具 ➔ 存入 `docs/proxy/` 或对应子目录
* 文件命名规范：建议采用小写英文字母 + 连字符（kebab-case），如 `modern/reality.md`。

### 步骤 2：在 `docs/_sidebar.md` 中挂载导航
Docsify 依赖 `_sidebar.md` 生成目录树。新增文档后必须在 `docs/_sidebar.md` 中添加对应行：

```markdown
* 某某分类
  * [文章标题显示名](相对路径/文件名.md)
```
> **注意**：如果不挂载到 `_sidebar.md`，文档将无法在左侧栏被用户发现和点击。

### 步骤 3：标记已淘汰/失效历史技术的规范
若某项技术已被 GFW 识别封锁或因平台政策失效，**请勿直接删除原文**（保留其历史演进参考价值），请在文件顶部插入标准警告卡片：

```markdown
> [!WARNING]
> **【历史技术归档 · 状态说明】**
> * **技术现状**：简要说明当前为什么不可用。
> * **失效根因**：从网络协议、DPI 特征、平台政策等角度给出技术剖析。
> * **权威文献与官方链接**：附上 USENIX 论文、官方公告或 GFW-Report 报告。
> * **现代替代方案**：引导读者阅读对应的新技术章节。
```

---

## 四、Git 协作与上游（Upstream）同步流程

### 1. 配置上游远程仓库（只需配置一次）

```bash
# 查看当前远程源
git remote -v

# 关联原作者主仓库作为 upstream
git remote add upstream https://github.com/hoochanlon/fq-book.git
```

### 2. 定期同步上游最新改动

```bash
# 获取上游所有更新
git fetch upstream

# 切换到本地 master 分支并合并
git checkout master
git merge upstream/master

# 推送同步到你自己的 fork 仓库
git push origin master
```

### 3. 向原作者发起 Pull Request (PR)

1. 在本地完成修改并推送到你的 GitHub：
   ```bash
   git add .
   git commit -m "docs: xxx"
   git push origin master
   ```
2. 登录 GitHub，进入 [Long-HaiPeng/fq-book](https://github.com/Long-HaiPeng/fq-book)；
3. 点击页面顶部的 **Contribute ➔ Open pull request**；
4. 填写本次修改的清晰说明，提交 PR 等待原作者 Review 与合并。

---

## 五、GitHub Pages 在线发布

本项目使用 GitHub Pages 进行零成本托管：

1. 进入仓库 **Settings ➔ Pages**；
2. **Source** 选择 `Deploy from a branch`；
3. **Branch** 选择 `master` 分支，文件夹选择 `/docs`；
4. 点击 **Save**；
5. 几分钟后，站点将自动发布在：`https://你的用户名.github.io/fq-book/`。
