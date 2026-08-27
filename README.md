# Aemeath 桌宠（aemeath-dsh）

![Aemeath 桌宠](assets/DSniang1.png)

DeepSeek Harness（DSH）Web 界面右下角的常驻**桌面宠物**挂件：Aemeath 形象 + DeepSeek API 余额 + 今日已用 + 每轮对话消耗统计。本项目基于 [dsh-whale-widget](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget)（MIT）二次开发：**换上了 Aemeath 形象与点击态表情、粉色主题、自定义音效（笑声/变身）与每轮完成音效开关**，余额/记账/峰谷定价等核心能力全部保留。是标准 DSH 插件包，可通过 `dsh plugin` 安装/卸载。

## 特性

- 🐾 **常驻自启**：随 DSH Web 界面每次打开自动出现（标准 DSH bundle 插件）
- 🎨 **Aemeath 形象**：常态 cut-out PNG + **点击换脸**（点击/按住切换 mclick 表情，松手恢复）
- 🌸 **粉色主题**：气泡、文字、菜单全套浅粉色 UI
- 💰 **余额**：60 秒自动刷新 + 点击宠物手动刷新；余额变化时数字**滚动动画**；瞬时网络抖动自动沿用最近余额不报错
- 📊 **今日已用**：两种模式任选（见下），显示今日消耗金额
  - **小鲸鱼记账（推荐，免令牌）**：不需要任何会话令牌，每次观测余额后用余额差值自动记账（`.aemds-usage.json`，跨天自动归零归档）
  - **实时·令牌**：填入平台会话令牌后直接调用平台用量接口，按**峰谷定价**（工作日高峰 9:00–12:00 与 14:00–18:00，其余空闲；2026-08-23 起周末全天按谷价）实时换算今日已用
- 💬 **每轮对话消耗统计**：监听本机会话事件，每轮对话结束后弹出本轮消耗金额（精确 usage，非估算）
  - 菜单可开关「每轮对话后自动显示消耗金额」；「自动关闭时间」可自定义秒数（填 0 表示不自动关闭）
- 🔊 **自定义音效**：
  - 点击音效（**按下播放、松开静音**），菜单「音效」可切换 **笑声 / 变身** 两组
  - **每轮完成音效**：每轮对话结束时播放，菜单「完成音效」独立开关控制，音量跟随「音量」滑块
- 🖱️ **拖拽 + 四边四分之一吸附**（左/右/上/下，角落可组合）；左吸附时整体**水平镜像翻转**
- 🧸 **按压 Q 弹**玩偶效果（按压时底部坐标不变）
- 🎚️ **汉堡菜单**（悬停宠物右上角出现）：大小滑块（0.6–2.5 倍）、音效切换、音量调节、用量模式、峰谷提示文案、气泡开关、每轮消耗开关与自动关闭时间、**完成音效开关**
- 💬 **随机台词**：点击气泡切换随机台词段（加权随机），再点一次关闭
- 📐 随浏览器窗口自动缩放；文字位置/字号与图片联动

## 目录结构

```text
aemeath-dsh/
├── package.json          # DSH bundle 插件元数据（包名 aemeath-dsh）
├── README.md             # 本文件
├── cordis.patch.yml      # 插件挂载声明
├── lib/
│   └── index.js          # 宿主侧插件本体（含前端挂件代码）
└── assets/
    ├── DSniang1.png      # Aemeath 常态形象（cut-out，气泡由代码绘制）
    ├── mclick.png        # 点击态表情
    ├── rua.gif           # 点击/气泡动效（当前为点击态图）
    ├── Ya1.mp3           # 点击按下音（「笑声」组，可替换）
    ├── finish.mp3        # 每轮完成音效
    ├── hensin.mp3        # 「变身」组点击音
    ├── Ya2.mp3 / D1.mp3 / D2.mp3  # 兼容旧音效组（前端已不播放松开音）
```

## 安装

### 方式 A：直接从 GitHub 安装（推荐）

无需本地克隆，一条命令安装：

```powershell
dsh plugin --profile web add github:xu-qichen/aemeath-dsh
```

说明：

- 装完后插件会出现在 DSH 的**插件管理页面**里，之后可以直接在页面里更新，无需再手动执行命令
- 网络环境需要代理时，先设置代理环境变量再执行
- 安装完成后重启 `dsh web`，再 F5 刷新浏览器

### 方式 B：本地安装（从当前仓库）

在**仓库根目录**（`package.json` 所在目录）执行：

```powershell
dsh plugin --profile web add link:.
```

说明：

- **`link:.` 表示链接当前目录**（仓库根目录本身就是插件包）。如果复制了仓库到别处，用绝对路径：
  ```powershell
  dsh plugin --profile web add link:D:\你的路径\aemeath-dsh
  ```
- ⚠️ 不要写 `link:.\aemeath-dsh` 这类带子目录的路径——仓库里**没有** `aemeath-dsh/` 子目录
- 安装完成后重启 `dsh web`，再 F5 刷新浏览器
- 如果之后移动了源码目录，重新执行一次 `dsh plugin --profile web add link:.<新路径>`；提示冲突就先 `dsh plugin --profile web remove aemeath-dsh` 再 add

### 方式 C：发布到 npm 后安装

```powershell
npm publish   # 在仓库根目录执行一次
dsh plugin --profile web add aemeath-dsh
```

### 给 AI 的安装说明（用 dsh 辅助安装时，直接复制给 AI）

```
请帮我安装插件 aemeath-dsh，来源是 GitHub 仓库 xu-qichen/aemeath-dsh。

步骤：
1. 确保 pnpm 可用（没有就先：npm install -g pnpm）
2. 在 Web profile 安装（任选一种来源）：
    首选，直接从 GitHub 安装：
     dsh plugin --profile web add github:xu-qichen/aemeath-dsh
   或从本地仓库链接安装：
     dsh plugin --profile web add link:.<仓库绝对路径>
3. 重启 dsh web，然后 F5 刷新浏览器

安装后验证：
- dsh --profile web --dump-config 应该能看到 aemeath-dsh 在 bundles 里
- curl http://127.0.0.1:3080/aemeath-dsh/balance.json 应返回 200 JSON（含 totalBalance）
- curl http://127.0.0.1:3080/aemeath-dsh/widget.js 应返回 200 JS

另外请检查 DSH 凭据里是否配置了 DEEPSEEK_API_KEY（没有就提示用户配置；DEEPSEEK_PLATFORM_TOKEN 可选）。
```

### 关于令牌（安装后必读）

> **默认不需要任何令牌。** 安装后只需配置 `DEEPSEEK_API_KEY`（拉取余额必需），「今日已用」会自动使用默认的**小鲸鱼记账**模式（余额差值本地记账），开箱即用。
>
> 「实时·令牌」模式用到的 `DEEPSEEK_PLATFORM_TOKEN` 是**可选的**，仅在你想要更精确的实时用量换算时才需要配置（获取方式：登录 platform.deepseek.com → F12 → Network 找到 `usage/by_api_key/amount` 请求 → 复制其 `Authorization` 值）。

## 卸载

```powershell
dsh plugin --profile web remove aemeath-dsh
```

## 素材说明（想换皮肤/音效看这里）

- **常态形象**：替换 `assets/DSniang1.png`（要求：PNG、透明背景 cut-out、建议 ≥400px 正方形）
- **点击态表情**：替换 `assets/mclick.png`（点击/按住时显示）
- **点击音效（「笑声」组）**：替换 `assets/Ya1.mp3`（按下播放，松开静音；菜单「音效」切「变身」则播放 `assets/hensin.mp3`）
- **每轮完成音效**：替换 `assets/finish.mp3`
- 音效要求：MP3、建议 ≤3 秒、越小越好（原文件 18-27KB）
- 换完素材**刷新浏览器即可生效**（服务端读盘下发）；改 `lib/index.js` 里的逻辑/台词/样式则需要重启 `dsh web`

> ⚠️ **素材版权提示**：仓库内音效（`Ya1.mp3`/`finish.mp3`/`hensin.mp3`）取自游戏角色配音片段，版权归原权利方（游戏公司/配音演员），**仅供个人学习研究使用**；如需公开分发或商用，请替换为自有或无版权音效。

## 验证

```powershell
dsh --profile web --dump-config | Select-String -Pattern "aemeath"

curl http://127.0.0.1:3080/aemeath-dsh/image.png
curl http://127.0.0.1:3080/aemeath-dsh/balance.json
curl http://127.0.0.1:3080/aemeath-dsh/size.json
curl http://127.0.0.1:3080/aemeath-dsh/last-turn.json
curl http://127.0.0.1:3080/aemeath-dsh/sound/finish.mp3
```

- `/aemeath-dsh/image.png` → 200 `image/png`
- `/aemeath-dsh/balance.json` → 200，含 `{"ok":true,"totalBalance":...,"currency":"CNY","todayUsage":...}`
- `/aemeath-dsh/size.json` → GET 返回配置；PUT 写入
- `/aemeath-dsh/last-turn.json` → 200，含最近一轮对话消耗 `{seq, turn, amount, tokens}`
- 浏览器 F5 后右下角出现桌宠

## 常见问题

- **挂件不出现**：确认 `dsh plugin add` 成功；`dsh --profile web --dump-config` 里能看到 `aemeath-dsh`；重启 `dsh web` 后 F5。
- **图片不显示**：确认 `assets/DSniang1.png` 在插件包内。
- **余额报「未配置 DEEPSEEK_API_KEY」**：去 DSH 配置凭据。
- **今日已用显示 --**：记账模式下需要先跑一次余额观测（60 秒内自动完成）；令牌模式需要配置 `DEEPSEEK_PLATFORM_TOKEN`。
- **没有声音**：确认 `assets/*.mp3` 在包内且菜单「完成音效」已勾选、音量不为 0。
- **本地开发改了代码不生效**：使用 `link:` 安装时，修改 `lib/index.js` 后重启 `dsh web`（ESM 模块缓存）；改 `assets/` 素材刷新浏览器即可。

## 自定义指南

- **台词**：`lib/index.js` → `RANDOM_GROUPS` 数组（加权随机台词组）
- **颜色**：`lib/index.js` → `WIDGET_JS` 里的 CSS 字符串（粉色系 `#e09bb0`/`#c76a8c`/`#fff7fa`）
- **气泡大小/位置**：`.dshwv-bubble` 规则的 `width/left/top` 与 `--dshw-u`
- **图片位置/比例**：`.dshwv-img` / `.dshwv-img-mclick` 规则的 `right/bottom/width/height`（常态与点击态分别控制）
- **余额刷新/气泡时长**：`REFRESH_MS` / `BUBBLE_MS` / `CLICK_MS` 常量
- **峰谷定价表**：`lib/index.js` 顶部 `PRICING` / `PEAK_HOURS` 常量

## 致谢与许可证

本项目基于 [MeteorNOX/DeepSeek-Balance-Whale-Widget](https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget)（MIT）二次开发，保留原插件的余额/记账/峰谷/每轮消耗等全部核心能力。许可证见 [LICENSE](LICENSE)（MIT，含原作者署名）。

- 插件代码：MIT
- Aemeath 形象与皮肤：作者原创
- 内置音效片段：版权归原权利方，仅供个人学习
