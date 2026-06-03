---
title: Antigravity IDE / 2.0 (MacOS) 深度配置与疑难解答指南
date: 2026-04-21
tags:
  - 工具
  - 教程
  - 网络配置
  - Antigravity
  - MacOS
  - Antigravity IDE
  - Antigravity 2.0
image: /articles/antigravity/cover.png
---

## 一、部署前的核心准备工作

在正式开始配置前，请务必确认您的软硬件环境满足以下基础条件：

1. **畅通的国际网络环境**：首要前提是您的本地网络已具备无障碍访问 Google 服务的条件（本指南默认您已掌握基础的代理工具使用方法）。
2. **状态正常的 Google 账号**：推荐使用订阅了 Gemini Pro/Ultra 的高级账号。普通免费账号可用于测试，但极易受限；若您的账号已出现 `403 Forbidden` 报错或处于封禁状态，将无法完成后续接入。

> **⚠️ 核心资产安全与风控警告：** 
> 官方对于低价合租、非常规渠道代充的 Gemini 订阅账号打击力度极大（官方标准定价为 $20/月）。若您的订阅来源异常，随时面临被官方清退的风险。此外，在配置和日常使用期间，**切忌频繁切换跨国/跨地区代理节点 IP**，IP 的剧烈波动是触发 Google 风控机制导致封号的最常见原因。请务必保持单一高质量节点长期稳定使用。

---

## 二、先识别版本：Antigravity IDE 与 Antigravity 2.0

Google 更新后，Antigravity 已经拆成多个产品。请先确认自己正在使用哪一个客户端，否则界面截图、扩展入口和排障判断会对不上。

1. **Antigravity IDE**：这是原教程主要覆盖的 VS Code 风格客户端，有编辑器、侧边栏、扩展面板、SSH 等传统 IDE 结构。
2. **Antigravity 2.0**：这是新的独立桌面应用，官方定位更偏 Agent 项目管理与多 Agent 编排，已经不是 VS Code 那种界面。
3. **Antigravity CLI / SDK**：CLI 是终端工具，SDK 是开发者工具链，本篇不展开讲。

> **重要提醒：** 本教程继续覆盖 Antigravity IDE 与 Antigravity 2.0 的网络、登录、验证问题；但所有涉及“扩展面板”“SSH 插件”“界面位置”的内容，默认以 Antigravity IDE 为准。2.0 用户请优先参考网络接管、账号登录和手机验证部分。

> **给老用户的重要提醒：** 如果您原来安装的是旧版 Antigravity，它更新后可能会变成 Antigravity 2.0；这是官方的新产品方向，不是安装包损坏。大多数想继续使用原来 VS Code 风格界面的用户，请安装或保留 **Antigravity IDE**。官方更新流程中可能会提示是否重新安装 IDE，建议需要旧界面的用户选择保留/安装 IDE。

官方下载入口：[https://antigravity.google/download](https://antigravity.google/download)。进入页面后不要只点最上面的 Antigravity 2.0，请向下找到 **Antigravity IDE** 区域，再选择 macOS 对应版本下载：

* Apple Silicon 芯片（M1/M2/M3/M4 等）：选择 **macOS - Download for Apple Silicon**。
* Intel 芯片 Mac：选择 **macOS - Download for Intel**。如果官网后续调整支持范围，请以下载页当前显示为准。

如果您只是想跟着本教程原来的截图操作，或者需要扩展面板、SSH、Antify 单应用规则，请优先安装 **Antigravity IDE**，不要误装成只面向新界面的 Antigravity 2.0。

如果您打开后看到的是完整编辑器、扩展市场和类似 VS Code 的左侧栏，请按 **Antigravity IDE** 理解；如果打开后是新的项目/Agent 工作台，请按 **Antigravity 2.0** 理解。

---

## 三、MacOS 网络环境深度配置（二选一）

**原理解析：为什么不能直接连？**
Antigravity IDE 和 Antigravity 2.0 的底层网络请求都可能不会完整接管 MacOS 的"系统代理"流量。这意味着，即便您的浏览器能上外网，软件本体依然可能处于"断网"状态。因此，我们必须通过特定手段（虚拟网卡或强制局域网分流）来接管其底层网络请求。

我们为您提供两套解决方案，**强烈推荐首选方案 A**，配置门槛最低且成功率最高。

### 方案 A：全局流量接管（Tun 模式虚拟网卡）—— 首选极简方案

开启代理工具的 Tun 模式（即在系统底层创建一张虚拟网卡），能强制电脑上的所有软件流量都走代理，这是目前最省心的方案。无论您使用 Antigravity IDE 还是 Antigravity 2.0，都建议优先尝试这个方案。

1. **开启 Tun 模式**：以主流的 Clash Verge 为例，请进入软件设置，**开启"虚拟网卡"或"Tun 模式"**（部分软件如 Clash X Pro 称之为"增强模式"），同时**关闭原本的"系统代理"**。右侧的分流模式请保持在"规则（Rule）"。
2. **环境净化与重启**：为了避免路由表冲突，请彻底退出电脑上正在运行的其他加速器、VPN 或多余的代理软件。重启您主用的代理工具，并使用浏览器打开 Google 首页测试。如果无法访问，说明虚拟网卡未能成功创建或存在路由冲突，请排查网络工具自身设置。
3. **修复权限（如遇报错）**：首次开启 Tun 模式时，MacOS 通常会请求管理员密码以安装底层依赖。如果在软件内点击开启后无反应或报错，请寻找旁边类似"扳手"的图标，手动触发依赖项安装。

> **提示**：如果方案 A 在您的 Mac 上配置成功且能顺畅使用，请直接跳至【第三部分】。若由于系统冲突导致 Tun 模式无法启用，再尝试下方的方案 B。

### 方案 B：应用级强制分流（Antify + 系统代理）—— 进阶备用方案

如果您排斥使用全局虚拟网卡，希望实现更精准的单应用代理，可以通过 Antify 工具对 Antigravity 进行定点强制接管。

1. **基础代理设置**：在您的代理软件中，**关闭 Tun 模式**，**开启"系统代理"**，并确保模式依然处于"规则（Rule）"。
2. **获取并安装 Antify**：前往官网 https://antifyapp.com/zh 点击下载。下载后双击 `.dmg` 文件，将 Antify 拖入 `Applications（应用程序）` 文件夹完成安装。
3. **授予系统权限**：首次运行 Antify 引导至"下一步"时，系统会弹出需要修改网络设置的权限请求。请点击蓝色按钮跳转至 Mac 的"系统设置"，找到对应的网络扩展开关予以放行，并保存。
   * *排错：如果未自动跳转，请依次进入 Mac 的 `系统设置 -> 网络 -> VPN与滤网`，找到 Antify 并手动允许。*
4. **配置本地节点（Proxies）**：
   打开 Antify，点击左侧菜单的 `Proxies`，然后点击右侧的 `Add Proxy` 进行本地端口绑定。
   * **名称 (Name)**：自定义（如：Antigravity Proxy）。
   * **协议类型 (Type)**：根据您的代理工具选择（通常为 `HTTP` 或 `SOCKS5`）。
   * **服务器地址 (Server)**：填写本机回环地址 `127.0.0.1`。
   * **端口号 (Port)**：填写您代理软件提供的本地监听端口（若不清楚，可在 Mac 的 `系统设置 -> 网络 -> 代理` 中查看具体数值）。

```copy-dashboard
本机地址 (Server): 127.0.0.1
默认端口 (Port): 7897
备用端口 (Port): 7890
```

   * 保存配置，呈现如下状态：
   
   ![Antify 保存好 Proxies 配置的截图](/articles/antigravity/macos/image1.png) 

5. **配置应用分流规则（Rules）**：
   点击左侧菜单的 `Rules`，选择 `Add Rule`。打开 Mac 的 Finder（访达），进入"应用程序"目录，将您实际使用的客户端图标直接拖入 Antify 界面中央的虚线框内。在 `Proxy Server` 下拉菜单中，选择您刚才创建的节点并保存。
   * 如果您使用的是 **Antigravity IDE**，请拖入 Antigravity IDE 对应的应用图标。
   * 如果您使用的是 **Antigravity 2.0**，请拖入 Antigravity 2.0 对应的应用图标。
   * 如果两者都安装且都会使用，建议分别添加两条规则，避免只代理了其中一个客户端。

   ![Antify 左侧选择 Rules 并点击 Add Rule 的界面截图](/articles/antigravity/macos/image2.png) 
   ![Antify 添加 Rule 时拖拽 App 的虚线框截图](/articles/antigravity/macos/image3.png) 
   ![Antify 添加完成 Antigravity 规则的截图](/articles/antigravity/macos/image4.png) 

6. **连通性校验**：回到 `Proxies` 界面，点击刚刚创建的节点左侧的 **测速 (Latency)** 按钮。若显示具体延迟毫秒数，证明链路已打通；若无反应，请仔细核对步骤 4 中的协议与端口号是否填错。
   > **⚠️ 核心注意**：采用此方案后，每次使用 Antigravity 时，都必须保持 Antify 软件处于后台运行状态。

---

## 四、账号安全接入与验证流程

网络基建完成后，我们将进行账号的绑定。受限于 Google 的安全策略，根据您的账号权重，可能会遇到不同的验证流程。

### 4.1 尝试标准网页授权

1. 启动您实际使用的应用程序：Antigravity IDE 或 Antigravity 2.0。
2. 软件会唤起您的默认浏览器，请按照网页引导，使用您的 Google 账号进行授权登录。
3. 若浏览器提示授权成功，且切回软件后能正常拉取到模型、进入项目或创建对话，说明标准登录通关。请直接跳至下方的【4.3 开始对话与手机验证】。

> **遇到阻碍？**
> 若授权网页白屏、无响应或频繁提示错误代码，请先分清是网络未接管、账号地区资格不通过，还是本地授权状态异常。请勿反复重试以免锁定账号；确认不是资格问题后，再采用下方的本地授权辅助方案。

### 4.2 进阶登录：Cockpit Tools 本地授权辅助

**原理解析与边界说明：**
Cockpit Tools 的作用是在您的本机环境中完成 Google OAuth 授权，并把合法 Token（身份令牌）写入 Antigravity IDE 的本地状态，适合处理浏览器跳转失败、客户端状态错乱、授权写入失败等问题。
但它不是万能强登工具：如果官方已经判定账号地区/资格不符合，或服务端继续二次校验失败，单纯写入 Token 也可能无法通过。
这属于**本地授权辅助操作**，并非高危的"服务器反向代理（如 Openclaw 泛用）"；真正危险的通常是把额度交给第三方服务器长期反代使用。

> 🔄 **新版工具说明：** Cockpit Tools v0.24.0 起已明确对齐官方 **Antigravity IDE** 客户端，会按官方重命名后的路径、用户数据目录、进程识别和 Language Server 元数据处理 IDE，并改为读写官方 `antigravityUnifiedStateSync.oauthToken` 状态。请务必下载最新版再操作。想沿用本教程原截图和 VS Code 风格界面的用户，优先选择 **Antigravity IDE**；不要把它和新的 Antigravity 2.0 混在一起操作。

1. **下载工具**：访问官方代码库 https://github.com/jlcodes99/cockpit-tools/releases ，下拉至 `Assets` 区域，优先下载 `Cockpit_Tools_0.24.0_universal.dmg` 这类 universal 版本的 Mac 安装包；如果后续版本号变化，请选择最新版里对应的 universal `.dmg`。
   ![Github 上 Cockpit Tools 下载列表的截图](/articles/antigravity/macos/image5.png) 

2. **安装与安全放行**：将图标拖入"应用程序"完成安装。
    > **Mac 特别提示**：由于是第三方开发者工具，首次运行会被 Mac 拦截。请打开 `系统设置 -> 隐私与安全性`，向下滚动找到被拦截的 Cockpit 提示，点击"仍要打开"并输入电脑密码放行。
    > 
    > **💡 如遇“已损坏无法打开”提示（隔离放行）：**
    > 如果双击打开时，macOS 弹出“Cockpit Tools 已损坏，无法打开”的警告，这通常是 macOS Gatekeeper 安全隔离限制导致的，并不代表文件损坏。如遇此损坏提示，请按以下极简步骤处理：

**第①步**：请先确认您的 Cockpit Tools 是从上方官方 GitHub Releases 下载源下载的。

**第②步**：如果确认来源安全，请在 Mac 上打开 **“终端 (Terminal)”** 应用。

**第③步**：复制并粘贴执行下方提供的隔离放行命令（直接点击卡片右侧的“复制”即可）：

```command-dashboard
标题: macOS 隔离放行命令 - Cockpit Tools
说明: 如遇应用提示已损坏，请复制并在终端执行此命令
命令: sudo xattr -rd com.apple.quarantine "/Applications/Cockpit Tools.app"
```

*(提示：执行该命令时需要输入您的 Mac 锁屏密码。在输入密码时屏幕上不会有任何星号或字符显示，这是 Unix 系统的密码安全保密机制，直接盲输完并按回车键确认即可。)*

**第④步**：命令行执行完毕后，重新双击打开 Cockpit Tools 即可。

**第⑤步**：如果此时依然提示无法打开，再去 Mac 的 `系统设置 -> 隐私与安全性` 中，向下滚动找到 Cockpit 提示并点击“仍要打开”完成最终安全放行。


3. **初始化设置**：打开 Cockpit Tools，点击左下角的齿轮/设置图标，将界面语言切换为"中文"。
   ![Cockpit Tools 界面左侧底部设置按钮的截图](/articles/antigravity/macos/image6.png) 

4. **绑定授权**：在左侧应用列表中选中 `Antigravity IDE` 或 `Antigravity` 图标。点击界面上方的"添加账号"（或蓝色的 `+` 号），选择 **"开始 OAuth 授权"**。新版 Cockpit Tools 的 Antigravity 集成主要面向官方 **Antigravity IDE**；如果您的目标是新式 Antigravity 2.0，请不要套用旧版 IDE 的截图和路径判断。随后在弹出的浏览器窗口中完成 Google 登录。
   ![Cockpit Tools 界面左侧 Antigravity 图标被选中的截图](/articles/antigravity/macos/image7.png) 
   ![Cockpit Tools 顶部刷新旁边的加号按钮截图](/articles/antigravity/macos/image8.png) 
   ![Cockpit Tools 添加账号弹窗中 开始 OAuth 授权 按钮的截图](/articles/antigravity/macos/image9.png) 

5. **唤醒客户端**：授权成功后返回工具，在账号卡片下方点击 **"播放 (启动)"** 按钮。
   ![Cockpit Tools 账号列表中点击播放按钮的截图](/articles/antigravity/macos/image10.png) 
   此时系统会弹窗提示"未找到应用程序路径"，请点击选择按钮旁边的 **刷新** 标识，随后点击 **保存**。新版 Cockpit Tools 已适配官方重命名后的 Mac 应用 `/Applications/Antigravity IDE.app`，也能从旧 macOS 路径配置迁移到当前路径。如果自动识别不准，请手动选择 **Antigravity IDE.app**。若您的目标是旧版 VS Code 风格界面，请不要误选成新的 Antigravity 2.0 应用。Antigravity IDE 将被强制唤醒或重启。
   ![未找到应用程序路径弹窗中 点击刷新/重置按钮的截图](/articles/antigravity/macos/image11.png) 
   ![未找到应用程序路径弹窗中 点击保存按钮的截图](/articles/antigravity/macos/image12.png) 

6. **验证载入**：观察客户端界面。Antigravity IDE 通常会在顶部或右侧出现模型切换下拉菜单；Antigravity 2.0 则可能表现为可以正常进入项目、创建 Agent 或发起对话。只要账号状态能被识别并进入可对话状态，即代表本地授权状态写入成功。若仍提示没资格或认证错误，请按 FAQ 里的三类报错继续判断，不要反复强行注入。
   ![Antigravity 界面顶部模型选择下拉框（如 Gemini 3.1 Pro）的截图](/articles/antigravity/macos/image13.png) 

### 4.3 激活对话与防机器人验证（必做）

为了对抗机器批量滥用，首次对话必须通过手机号二次核验：

1. 在对话框中随意发送一句测试文本，界面右下角会拦截并弹出蓝色的验证按钮（`Complete verification`）。
   ![Antigravity 界面右下角提示 Verification required 的弹窗及蓝色按钮截图](/articles/antigravity/macos/image14.png) 
2. 点击蓝钮，在弹出的网页中选择第二个验证选项。
3. 国家代码切换至 `+86`（中国），输入您的手机号获取短信验证码。
4. **验证说明**：输入验证码后即可永久解锁对话权限。若提示"此号码验证次数已达上限"，属于 Google 对单个手机号的常规限制，请借用家人朋友的手机号接收即可（该号码仅做一次性风控核验，不涉及账号绑定）。
5. **报错 400 处置**：若点击蓝色按钮后网页提示 400 错误，表明当前浏览器的登录环境与软件不同步。请在 Mac 设置中将默认浏览器切换为您刚登录 Google 账号的那个浏览器，或在该默认浏览器中重新登录一次对应账号。

---

## 五、极客专属：SSH 远程开发配置（非必选）

> **技术建议**：本节主要面向 Antigravity IDE。Antigravity 2.0 已经是新的独立 Agent 桌面应用，不再是传统 VS Code 风格 IDE；如果您使用的是 2.0，请优先查看它自己的项目、权限和终端相关功能，不要强行套用旧版扩展面板截图。普通用户请直接跳过本节。

为解决 SSH 直连的网络穿透问题，请执行以下适配步骤：

1. **获取专属插件**：访问 https://open-vsx.org/extension/dinobot22/antigravity-ssh-proxy ，点击右下角紫色的 `Download` 下载 `.vsix` 扩展包。
2. **安装扩展**：打开 Antigravity，点击左侧边栏的"扩展"管理面板，将刚才下载的包直接拖拽进去完成离线安装。
3. **配置终端端口**：安装后，客户端底栏会多出一个绿色的 `ATP` 标识。点击它，在弹出的输入框内填入您本地代理工具的监听端口号并保存，随后再执行 SSH 服务器连接指令。
   > **网络逻辑闭环提醒**：如果您当前使用的是【方案B: Antify应用代理】，此处的 ATP 扩展可以直接生效。但如果您使用的是【方案A: Tun模式】，在进行 SSH 握手时，**必须同时勾选代理软件的"系统代理"开关（即 Tun 与系统代理双开）**。连接稳固后，方可关闭系统代理。
4. **远端注入**：在成功连接 SSH 的终端窗口侧边栏，再次进入"扩展"面板，找到类似 `在 SSH:XXX 中安装扩展` 的选项。手动为远端服务器环境安装 `Antigravity SSH Proxy`。
5. **生效**：彻底关闭并重启当前工作区窗口，SSH 代理链条即可完全贯通。

### 扩展市场加载慢 / 搜不到插件怎么办？

> 💡 **适用范围：** 这一段只针对 **Antigravity IDE** 的扩展面板。Antigravity 2.0 不是传统 VS Code 风格 IDE，请不要强行套用。

如果扩展面板默认提示正在使用 **Open VSX**，可能会出现插件搜索不全、下载很慢、甚至加载失败的情况。可以把扩展市场源切换为 VS Code 官方源。

![扩展面板提示当前使用 Open VSX](/articles/antigravity/marketplace-open-vsx-notice.png)

操作方法：

1. 打开左侧 **扩展（Extensions）** 面板。
2. 如果页面提示 `By default, Antigravity uses Open VSX as a marketplace. This can be changed in Antigravity settings.`，点击里面的 **Antigravity settings**。

   ![点击 Antigravity settings 进入扩展市场设置](/articles/antigravity/marketplace-settings-link.png)

3. 在 `Editor -> Marketplace` 设置里，把下面两个地址分别填入对应输入框。

   ![Marketplace 设置里的 Item URL 与 Gallery URL 输入框](/articles/antigravity/marketplace-url-settings-empty.png)

```copy-dashboard
Marketplace Item URL: https://marketplace.visualstudio.com/items
Marketplace Gallery URL: https://marketplace.visualstudio.com/_apis/public/gallery
```

   ![填入 VS Code 官方扩展市场地址后的效果](/articles/antigravity/marketplace-url-settings-filled.png)

填完后，**彻底关闭并重新打开 Antigravity IDE**，再回到扩展面板搜索 `ssh`、`Remote - SSH` 等插件。正常情况下，搜索结果会切到 VS Code 官方插件市场，插件会更全，加载也更稳定。

---

## 六、常见疑难杂症 (Troubleshooting)

### 异常 0：为什么我打开的 Antigravity 2.0 和教程截图完全不一样？
* **原因**：Antigravity 2.0 是新的独立桌面应用；原教程截图更接近 Antigravity IDE，也就是旧版 VS Code 风格客户端。
* **对策**：网络接管、登录、手机验证仍可参考本教程；但扩展、SSH、侧边栏位置、模型下拉框位置可能不同。若您需要原来的 IDE 体验，请确认自己安装的是 Antigravity IDE。

### 异常 0.5：登录时报错，应该先判断哪一类？

现在 Antigravity 的登录校验比旧版更严格，不建议一看到失败就反复用 Cockpit Tools 注入。请先按截图判断类型：

1. **提示账号没资格：优先处理地区/账号资格**

   如果看到 `Sorry, this account is ineligible to use Antigravity`，这通常不是普通 Token 写入问题，而是账号资格、地区或官方准入校验没有通过。

   ![账号无资格使用 Antigravity 的提示](/articles/antigravity/auth-ineligible-account.png)

   处理建议：先检查 Google 账号地区、付款资料地区、订阅状态和当前代理节点地区是否一致。此类问题目前不能再简单理解为“Cockpit 强制注入即可解决”；必要时只能调整地区环境或更换符合资格的账号。

   如果页面提示 `Sorry, this account is ineligible to use Antigravity`，并且下方显示 `Authentication failed`，可以按下面方式尝试修改 Google 账号注册地：

   1. 先打开 Google 条款页查看当前账号关联地区：https://policies.google.com/terms。页面底部通常会显示当前账号对应的国家/地区；如果显示的是中国大陆、中国香港、中国澳门等受限地区，就需要继续处理。
   2. 打开 Google 账号地区修改申请页：https://policies.google.com/country-association-form。
   3. 将账号地区申请修改为 Antigravity 支持的地区，例如美国、日本、新加坡等。若原本已经是美国但仍提示地区不可用，可尝试改为其他常用节点所在地区。

   ![Antigravity 支持地区参考](/articles/antigravity/supported-regions.jpg)

   4. 理由选择“其他”，说明自己因工作需要使用 Gemini / Antigravity，需要更新账号地区。
   5. 提交后等待邮件通知，通常需要约 24 小时。地区生效后再重新打开 Antigravity 测试登录。

   可参考英文理由：

```text
I'm using Gemini for work and I need to update my location to this US address: 2792 Bascom Corner Road, Rising Sun, IN 47040
```

   注意：Google 账号地区修改通常一年只能申请一次，提交前请确认目标地区和你长期使用的代理节点地区尽量一致，不要频繁乱改。

2. **提示 `oauth2.googleapis.com/token` 连接失败：优先修网络**

   如果报错里出现 `Post "https://oauth2.googleapis.com/token"`、`connectex`、`failed to respond`、`connection attempt failed`，说明客户端请求 Google OAuth token 接口时没有走通网络。

   ![OAuth token 接口连接失败，通常是网络未接管](/articles/antigravity/auth-oauth-network-timeout.jpeg)

   处理建议：回到第三章检查 Tun 是否真的开启、代理节点是否可用；如果使用 Antify，请确认 Antigravity IDE / Antigravity 2.0 应用本体已经加入规则。这个问题的重点是“网络没走上代理”，不是账号没资格。

3. **提示认证错误：再考虑 Cockpit 本地授权辅助**

   如果客户端里显示 `There was an error with your authentication. To log in, click here`，更像是本地授权状态、Token 写入或客户端认证状态异常。

   ![客户端提示 authentication 认证错误](/articles/antigravity/auth-token-injection-error.png)

   处理建议：可以使用 Cockpit Tools 重新完成 OAuth 并写入本地状态。但新版官方仍可能继续检测账号资格、地区和 Token 状态，所以 Cockpit 只能作为本地授权辅助，不保证绕过官方服务端校验。

### 异常 1：点击发送消息后，右下角按钮一直呈灰色且无回复
* **诊断**：典型的客户端断网表现，网络接管彻底失败。
* **对策**：请从头复盘第三部分的【网络环境深度配置】。正常连通的标志是：消息发送几秒后，右下角的发送图标会变成"带红色方框的停止生成按钮"或进入生成状态。

### 异常 2：模型列表能刷出，但对话无限加载卡死
* **诊断**：网络处于半连接状态，通常是代理节点自身网络不稳，或分流规则发生了内耗冲突。
* **对策**：首先尝试切换一个更干净的代理节点；若无效，请彻底放弃当前使用的网络接管方案，改用本教程中的另一套方案重置网络环境（例如：由 Tun 模式切换为 Antify 模式）。

### 异常 2.5：Cockpit Tools 更新后，应该选 IDE 还是 2.0？
* **先下载最新版**：请从 [Cockpit Tools Releases](https://github.com/jlcodes99/cockpit-tools/releases) 下载最新安装包。v0.24.0 起，Cockpit Tools 已将 Antigravity 集成对齐到官方 **Antigravity IDE**。
* **新版改善**：新版会识别 `/Applications/Antigravity IDE.app`，并读写官方 `antigravityUnifiedStateSync.oauthToken` 状态，切号/导入比旧版更贴近官方客户端。
* **排错重点**：需要原来 VS Code 风格界面的用户，请选择 **Antigravity IDE**。如果 Cockpit Tools 自动识别到了新的 Antigravity 2.0 应用，但您想跟着本教程原截图操作，请手动切回 Antigravity IDE 应用。
* **2FA 提示**：v0.24.0 也给 Codex OAuth 授权加入了 2FA 快速取码入口，但这是 Codex 相关能力；Antigravity 登录仍以本节的 Google OAuth 和客户端注入流程为准。

### 异常 3：发起对话即刻报错，且右下角弹出蓝色 `Retry` 按钮
此为高频综合型报错，核心诱因通常有以下四点，请逐一排查：

1. **账号未满 18 岁（年龄限制拦截）**
   * *表现*：在 Cockpit 的账号列表中，右上角若出现红色的 `unknown` 标识，基本确诊。
   * *对策*：访问 Google 账号年龄验证专线 https://myaccount.google.com/age-verification?utm_source=p0，完成实名/信用卡年龄验证。完成后等待 5 分钟，在 Cockpit 刷新状态，变为 `free/pro/ultra` 后重新注入登录。
2. **多开工具引发的数据串联**
   * *诊断*：若您曾在本机使用过其他第三方开源工具登录过该账号，残留的 Session 易导致状态机混乱。
   * *对策*：清空相关缓存，严格只通过 Cockpit Tools 作为唯一入口进行统一调度。
3. **Gmail 服务冲突（关联账号污染）**
   * *诊断*：有些非 Gmail 结尾的 Google 账号（如用国内邮箱注册），在无意中开通了附属的 Gmail 服务，导致认证身份发生漂移。
   * *对策*：进入 Google 账户设置 -> 数据和隐私设置 -> 下载或删除您的数据 -> 点击"删除 Google 服务"，将衍生的 Gmail 服务彻底移除，恢复原始邮箱的纯净状态即可。
4. **终极原因：账号被官方封杀**
   * *诊断*：若穷尽上述所有方案均告失败，遗憾地说明该账号已触发 Google 严重红线被执行封禁。近期官方风控趋严，请规范用车。

---

## 七、技术支持与结语

遵循上述模块化流程，99% 的接入异常均可迎刃而解。只要网络基建扎实，配置过程其实是一劳永逸的。如果您在实操中依然屡屡碰壁，通常归结于您的系统环境极为特殊，或对基础软路由逻辑存在误区。

如需更加深度的排障支持，我们也提供一对一的远程技术协助服务，由资深工程师亲自为您调优网络环境。

### 售后群二维码

如有任何问题或需要技术支持，请扫描下方二维码加入售后群：

![售后群二维码](https://sxxxxxxxxxxxxxxxxxxxxx.github.io/picx-images-hosting/售后群二维码.webp)
