# Knowledge Base - Release

本地优先的知识库桌面应用（Tauri 2.x + React 19）的安装包与自动更新端点仓库。

## 最新版本: v1.50.0

| 平台 | 下载链接 |
|------|---------|
| Windows x64 | [Knowledge.Base_1.50.0_x64-setup.exe](releases/v1.50.0/Knowledge.Base_1.50.0_x64-setup.exe) |
| macOS Apple Silicon | [Knowledge.Base_1.50.0_aarch64.dmg](releases/v1.50.0/Knowledge.Base_1.50.0_aarch64.dmg) |
| macOS Intel | [Knowledge.Base_1.50.0_x64.dmg](releases/v1.50.0/Knowledge.Base_1.50.0_x64.dmg) |
| Linux x64 (deb) | [Knowledge.Base_1.50.0_amd64.deb](releases/v1.50.0/Knowledge.Base_1.50.0_amd64.deb) |
| Linux x64 (AppImage) | [Knowledge.Base_1.50.0_amd64.AppImage](https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/v1.50.0/Knowledge.Base_1.50.0_amd64.AppImage)（R2 CDN，>100MB 未入 git） |

> ⚠️ **v1.31.0 的 Windows 用户请升级到 v1.31.1**：v1.31.0 所用的代码签名证书被 ESET 等杀软列入证书黑名单
> （报 `Win32/CertBL`），可能导致应用文件被误删。v1.31.1 已更换签名证书，功能与 v1.31.0 完全一致。

> 📱 **Android（移动端）走独立版本线**（从 0.1.0 起，与桌面 1.x 解耦），下载与版本历史见下方「[移动端（Android）](#移动端android)」。

## 自动更新

应用内自动更新走多端点容错（桌面 = `update.json`，移动端 = `update-mobile.json`）：

| 优先级 | 桌面端点（update.json） | 说明 |
|-------|------|------|
| 1 (主) | `https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/update.json` | Cloudflare R2 CDN，国内快 |
| 2 (备) | `https://github.com/bkywksj/knowledge-base-release/raw/main/update.json` | GitHub raw 兜底 |
| 3 (兜底) | `https://gitee.com/bkywksj/knowledge-base-release/raw/master/update.json` | Gitee raw |

移动端把上表三个 URL 里的 `update.json` 换成 `update-mobile.json` 即可。

## 移动端（Android）

Android 版有**独立版本线**（从 0.1.0 起），与桌面 1.x 解耦——可单独发布、单独「检查更新」。
APK 用固定 keystore（`kb-release.jks`）正式签名，CI（`android.yml`，推 `mobile-v*.*.*` tag 触发）自动构建（侧载 APK + Google Play AAB）。

| 移动版本 | 下载 |
|------|------|
| **0.1.0** (2026-05-12) | [Knowledge.Base_0.1.0_android-arm64.apk](releases/mobile-v0.1.0/Knowledge.Base_0.1.0_android-arm64.apk) ｜ [.aab](releases/mobile-v0.1.0/Knowledge.Base_0.1.0_android-arm64.aab) ｜ [R2 稳定链接（mobile-latest.apk）](https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/mobile-latest.apk) |

应用内「检查更新」：「我的 → 检查更新」读 `update-mobile.json`，发现新版本给出 APK 直链 → 浏览器下载 → 点一下进系统安装器（首次需在系统设置里允许「安装未知应用」）。

> ⚠️ 同一条版本线的 APK 必须用同一个签名 keystore，否则「检查更新→装新 APK」会因签名不匹配失败（`INSTALL_FAILED_UPDATE_INCOMPATIBLE`，只能卸载重装）。

### 移动端版本历史

#### mobile-v0.1.0 (2026-05-12)

**移动端（Android）首个正式版**

独立 APK 可侧载，App 内「检查更新」自助升级（读 `update-mobile.json`）。功能覆盖：Markdown 编辑（源码 ↔ 渲染预览切换）/ 全文搜索 / 双向链接 / 标签 / 回收站 / AI 问答（含「针对当前笔记问 AI」）/ 闪卡复习 / 任务 / 闪念捕获 / 每日笔记热力图 / 相机扫码 / WebDAV 手动推拉 / 单文件导入 / 网页剪藏；跨设备配置分享（WebDAV 源 / AI 模型 / ASR 配置，可 PIN 加密 PBKDF2+AES-GCM-256）。kb-release.jks 正式签名，`android.yml` 自动构建。

## 版本历史

### v1.50.0 (2026-08-03)

**整页白板 + 笔记内嵌白板块 + 历史日记导入 + 跨平台数据搬迁修复**

新功能：
- **整页白板**：新建画布类型笔记，基于 Excalidraw（已完全离线化，不连任何外部服务），
  支持导出图片，在笔记列表里作为独立类型区分显示。
- **笔记内嵌白板块**：在 Markdown 正文里直接插入画布，平时以图片形式显示，双击展开编辑，
  改完自动回写。白板内容已接入同步与双向链接体系。
- **历史日记导入**：支持「日期文件夹 / 笔记.md」这种目录结构直接导入为日记，
  已导入过的笔记会自动认回对应日期，不重复建条目。
- **笔记底部常驻链接状态条**：链出 / 链入 / 断链一次看全，替换掉原工具栏里的链接按钮。
- **窗口大小与位置记忆**：关闭时记住窗口几何，下次原样恢复；设置里提供「恢复默认大小」。

优化：
- 主窗口启动高度系数从 0.77 提到 0.88 —— 1080p 屏上默认高度从 832 抬到 950。
- 链接状态条与纸张卡片左右对齐，补上此前缺失的背景层。

修复：
- **跨平台数据搬迁的裂图与拉取失败**：路径分隔符归一化 + 数据体检自动修复 + 指针守卫，
  Windows ↔ macOS / Linux 之间搬数据后不再出现图片裂开、同步拉取中断。
- **`attachments/` 存量附件漏同步**：schema v54 清空附件扫描标记，旧附件会被重新纳入同步。
- **坏字节拖垮整次同步**：TEXT 列改用 UTF-8 降级读取，个别损坏字节不再让整次同步失败。
- **日记双链单向失效**：日记此前不同步出链，导致双向链接只有一个方向生效。
- **设置写盘改增量**：副窗口不再用陈旧状态覆盖主窗口刚改的设置。
- 单实例插件不再区分 dev / prod，装了正式版也能正常跑 `tauri dev`。

### v1.32.0 (2026-07-31)

**AI 写作不再混入思考过程 + 字体可自定义 + 大笔记打印提速**

编辑器：
- AI 写作辅助不再把推理模型的「思考过程」替换进笔记 —— deepseek-r1 / qwq / qwen3 这类模型
  经本地 Ollama / LM Studio 时会把 `<think>…</think>` 混在正文里，此前会被整段插入笔记。
  现在思考过程单独折叠展示，替换 / 追加只写入正文
- 标题字体可在设置里单独指定（默认跟随正文）；工具栏新增「字体」下拉，可对选中文字设字体，
  也能一键清除从 Word / 网页粘进来的内联字体
- 双链右键菜单加「修改链接」；修复表格内双链点不动、跨节点不识别、日记页点击无响应
- 修复粘贴 Windows 路径时反斜杠被 Markdown 转义吃掉
- 修复切换笔记后标题折叠失效、有序列表引线压住编号

导出与打印：
- 导出 HTML / Word 带上标题编号
- 导出 HTML / 打印 / Word（装了转换器时）跟随设置里的正文与标题字体
- 大笔记打印 / 导出提速：砍掉几十 MB 的进程间往返、图片内嵌限并发并封顶，全程带进度提示

其他：
- MCP HTTP 服务 401 返回可操作提示，设置页支持一键复制连接配置
- 修复重启后设置丢失（单实例守护 + hydration guard）

### v1.31.1 (2026-07-28)

**更换 Windows 代码签名证书（功能与 v1.31.0 完全一致）**

v1.31.0 所用的代码签名证书被 ESET 等杀软列入**证书黑名单**（检测名 `Win32/CertBL`），
命中的用户会看到「威胁已删除」提示、应用文件被直接删除，导致程序无法启动或部分功能失效
（PDF 预览、OCR、MCP 服务）。这不是应用本身的问题，而是签名证书被整体封禁所致。

v1.31.1 已更换为新的 EV 代码签名证书重新签名，**未改动任何功能代码**。
受影响的 Windows 用户请下载安装本版本；若杀软此前删过文件，重新安装即可恢复。

### v1.31.0 (2026-07-28)

**MCP HTTP 服务 + 笔记双树页签 + 文件夹自动导入 + 任务放弃状态**

新功能：
- MCP HTTP 服务：把自家知识库暴露给外部 agent 调用
- 笔记面板加「文件夹 / 标签」页签，两棵树合并到一处
- 文件夹自动导入：盯住一个目录，新落地的 `.md` 自动进库
- 任务新增「放弃」状态（事情黄了但想留个记录）；日历支持跨天条 / 分类配色 / 子任务时间 / 日期段选择
- AI 对话支持角色预设（复用提示词库）；上下文预算统一按模型窗口算，少截断多给上下文
- 笔记工具栏「格式规整」、右键「在新窗口打开」、源文件下拉「打开所在文件夹」
- 标题编号改用 JS 计算引擎（修复折叠后编号错乱）；自带手写编号的标题加跳号提示

修复：
- 同步健壮性加固：消灭「同步卡死」与「导入写坏库导致打不开」
- 改过名的 PDF / `.doc` 附件预览失败
- 表格单元格内代码 / 多行内容保存后被拆碎
- 粘贴含 base64 内联图的 Markdown 显示成源码
- 代码块命名存得进读不回，且会串进语言框
- 大纲条目点不动 + 编辑区两侧空白过大
- AI 问答存为笔记不再显示 Markdown 源码
- 原生 title 悬停提示统一为深色气泡

### v1.30.0 (2026-07-14)

**编辑器自选字体 + 笔记切换体验优化 + 视频区间播放**

新功能：
- 编辑器正文可自选系统已安装的任意字体（可搜索下拉，每项用该字体自身预览；未装自动 fallback 防乱码）
- 视频时间戳支持 A→B 区间：两次打点圈定片段，播到终点自动暂停
- 笔记卡片模式显示图片缩略图；卡片预览剥离 Markdown 噪声更干净
- 日记：桌面编辑器新增大纲面板；放开未来日期选择
- 笔记列表右键新增「复制笔记 ID」；关于页赞赏码放大并支持点击全屏预览

体验 / 性能：
- 切换笔记保持浏览位置：切走再切回停在原来阅读处，不再回到开头
- 消除切换时的整屏重载闪烁与卡顿（编辑器保持挂载、原地换内容 + 内容锚点式滚动恢复）

修复：
- 表格单元格含代码块保存后重开损坏（回退 HTML 序列化）
- 附件文件名含空格导致链接渲染失败与漏同步
- 坚果云 WebDAV MOVE 409 DuplicateName 降级直接 PUT
- AI 对话发送后显示「正在分析」占位 + 防连点重复发送；流式解码防中文乱码 / 丢 token
- pdf-extract 可恢复 panic 不再误报崩溃弹窗；打开笔记「Rendered more hooks」崩溃（TabBar hook 顺序）

### v1.20.0 (2026-07-02)

**本地 OCR + MCP 插件系统 + 大量编辑器/待办改进**

新功能：
- 本地 OCR：内置 RapidOCR 离线识别图片 / 扫描件 PDF（Win/Linux sidecar，macOS 用 Apple Vision 零二进制）；导入扫描件 PDF 自动 OCR 兜底
- MCP 插件系统：清单一键安装 + 脚手架生成（任意语言写插件，进程隔离）
- kb-mcp CLI：search / get / tags / recent / tag 命令行查询，省 token
- MCP 升级 v1.0：外部写入自动刷新列表 + 27 工具白名单裁剪
- 单篇笔记导出为单文件 Markdown（图片内嵌，不生成文件夹）
- 标题自动编号 + 彩虹色；复制为 Word（富文本）
- 待办默认视图可持久化；新增 KIMI (Moonshot) AI provider

编辑器改进：
- 双链单击即跳转；有序 / 无序列表可转标题；右键复制笔记 ID
- 插图不再打断有序列表序号；复制纯文本不再带 Markdown 标记（粘到记事本干净）
- 代码块统一字号 + 布局预设 + 大纲左右位置；搜索跳转直达命中位置
- tab 右键加「关闭全部」

修复：
- 根治静默闪退（全局 panic hook + 启动容错 + 前端四层兜底）
- AI 流式解码改字节缓冲，修中文乱码 / 丢 token；取消映射防并发串键
- 打印长笔记只出一页；幻灯片模式外链跳走；子任务连续录入丢焦点；紧急提醒响铃封顶 5 分钟
- 日历视图自适应 + 首页 / 待办全屏宽度跟随

### v1.15.0 (2026-06-23)

**笔记侧栏搜索 + AI 附加日记 + 多项打磨**

- 笔记侧栏就地搜索框；AI 对话可附加日记
- 列表右键复制内容；同步源分享支持全部类型；预置小米 MiMo 模型
- 优化：打开笔记提速、幻灯片表格不再显示成裸 HTML、日记日期时区统一、表格超宽横向滚动、退出同步不阻塞主窗口

### v1.14.0 (2026-06-11)

**编辑器公式 + 所见即所得打印 + 文件夹级联删除 + 布局/同步/AI 打磨**

- 编辑器工具栏新增公式（LaTeX）下拉，一键插入行内 / 块级公式
- 新增「所见即所得」打印 / 打印成 PDF
- 标题填完按回车自动聚焦正文末尾，方便接着写
- 修复图片未选中态超出编辑框（保存重进后宽图溢出）
- 非空文件夹支持级联删除（笔记进回收站，可恢复）
- 首页待办速览改 4 段，无截止日期 / 未来任务也显示
- 首页推荐新增 Sigil / Reeve / AgileShot 三款工具卡片
- 修复删任务后侧边栏红色角标不清零（待办统计排除软删墓碑）
- WebDAV 对不支持 MOVE 的服务器降级直接 PUT
- AI 智能模式下「附加文件夹」对工具调用同样生效，不再读到范围外笔记
- 修复单击托盘图标不隐藏窗口（Windows is_focused 误判）
- 桌面端忽略窗口宽度，竖屏 / 双屏 / 分屏不再误切移动界面

### v1.13.0 (2026-06-01)

**自动更新后台预下载 + 待办项目管理 + 知识图谱文件夹层级 + 编辑器与交互打磨**

- 后台预下载更新，秒装重启，重启弹窗展示更新内容
- 新建待办的项目下拉支持内联新建 + 管理项目
- 知识图谱纳入文件夹层级（folder 节点 + 层级边）
- 笔记文件夹支持「启动默认收起」开关
- 大纲默认变窄，隐藏大纲时正文自动补宽
- 修复图片后空行无限增长、打开即提示保存
- 修复粗体+高亮 markdown 往返后高亮丢失
- 修复右键菜单过长时被窗口底部裁掉

### v1.12.0 (2026-05-29)

**笔记推送/发布 + 快速记一笔 + AI 检索范围限定 + 编辑器交互打磨**

笔记推送/发布：
- 新增**推送页**（`/push`）+ 推送任务表单 + **定时推送**，可把笔记推送/发布出去
- 定时推送结果**居中弹窗**展示，弹窗内容支持 **Markdown 渲染**（加粗/列表/标题等）

效率工具：
- **「快速记一笔」独立悬浮窗** —— 随手快速捕获，不打断当前工作
- **编辑器高亮快捷键自定义** —— 在设置里自定义高亮快捷键

AI 增强：
- **AI 对话支持限定 RAG 检索范围到指定文件夹**（含子孙文件夹）
- **AI 回答下方溯源展示引用笔记里的图片**
- Ollama 走 OpenAI 兼容端点 + length 截断容错

编辑器与界面：
- 版面观感优化（阅读列宽 / 纸张质感 / 纹理 / 首行缩进）
- 图片双击放大 + 粘贴 data URL 自动落盘 + 查找栏吸顶 + 分栏 HTML 导出
- 表格交互增强 + 侧边栏多选 + 图片导入嗅探
- AI 菜单改为贴近选区悬浮，一行展开所有提示词
- 笔记面板：文件夹单击切换选中、双击才展开折叠
- 日记：侧边栏列表显示标题、改标题后刷新
- 任务：新建任务支持草稿子任务 + 紧凑模式子任务列表

修复与其他：
- 修复 Word 导出 HTML 内容丢失（只剩标题）
- 单 .md 导入识别同级 `attachments/` 中的图片
- 同步 V0 ZIP 导入加 ZIP slip 防护 + V1 远端删除改 edit-wins
- 移除多开实例功能，仅保留单实例守护
- rustc 线程栈调至 32MB 防 generate_handler 栈溢出

### v1.11.0 (2026-05-23)

**任务管理大升级 + 编辑器富化 + 笔记跨端同步全量打通**

任务管理：
- **工作流看板**（按 todo / doing / done 三列）+ **四象限视图**（重要×紧急自动分组）+ **项目甘特图视图** —— `/tasks` 顶部一键切换列表/看板/四象限/日历/甘特
- **AI 智能规划**：输入今日目标 → AI 按四象限自动拆分待办，弹 Modal 让你勾选/编辑/保存
- **任务 + 项目 + 任务分类跨端同步**（同步 v44 schema 升级）：projects/tasks/task_categories 全部走 stable_uuid + tombstone 软删 + last-write-wins 仲裁，两台电脑改任务互不丢

编辑器：
- **思维导图增强**：PNG 导出 / 全屏 / 独立弹窗
- **笔记内幻灯片演示模式**：用 `---` 分页，按方向键翻页，全屏黑底
- **Dataview 块 v0.1**：笔记内嵌实时数据视图
- **Word/Excel/文本附件应用内预览**（不用跳外部程序）
- **树形标签**：标签支持父子层级，侧栏可折叠展开

笔记 / 标签 / 同步：
- **Obsidian 嵌套标签 + 行内 `#tag` 提取**：导入 Obsidian 库时不再丢标签层级
- **WebDAV 同步**：7 项数据丢失/一致性缺陷修 + 2 项可感知性缺陷修 + sync_remote_state 死行 GC
- **pull 后自动重建反向链接**（之前同步完反链不显示）
- **AI 配置跨软件互通**：ai.profile 通用协议，与系列其他应用共享模型配置
- **日记面板视图切换** + 文案统一「每日笔记 → 日记」

性能 / UX：
- **文件树 > 200 节点启用 virtual scroll**（千级笔记不再卡帧）
- **外部 .md 首次打开弹引导**：让用户知道编辑会写回原文件 + 加入本地库
- **默认窗口宽度 1330 → 1388**：编辑器顶部 toolbar 一行显示
- 编辑器搜索 Ctrl+F 不再需要先点正文 + 工具栏入口

Bug 修：
- **AI 回复**：表格 / 删除线 / 任务列表不渲染（remark-gfm 接入）
- **反向链接**：遇到 TaskItem 序列化转义时失效
- **幻灯片**：之前永远 1/1 一页（DOMParser 解析 markdown 失败）
- QQ 群反馈 4 个 bug 全部修复（zip 导入热重载 db / 文件夹层级递归 / 外部 md 引导 / 文件树性能）

### v1.10.0 (2026-05-15)

**编辑器体验全面提升 + 日记/快速记一笔**

- **AI 写作工具栏**：从"跟随鼠标的浮窗"改为"钉顶 sticky bar"，与主工具栏视觉融合，不再被豆包/划词翻译这类系统级浮窗遮挡
- **斜杠菜单新增「日期与时间」分组**：`/jt`（今天）、`/xz`（现在）、`/sj`（当前时间）、今天 + 星期等 4 个预设，纯前端、零延迟
- **代码块文件名 / 自动换行 / 行号设置可随笔记保存**：Docusaurus 风格 fence info（```python title="X" wrap no-line-numbers），读回时自动拆分到对应 attr
- **「未分类」不再混杂日记**：日记纯净走「每日笔记」面板（已有月历视图），未分类只显示真正手动散记
- **「快速记一笔」**：`Ctrl+Alt+N` 弹小输入框，Enter 保存，每条作为带 🕐 时间戳的 callout 块追加到今天的日记 —— 一键速记 + 自动归档 + 日内时序回顾的闭环
- AI 工具栏初版"锚定选区"试做也在本版（被 sticky 顶部 bar 方案替代，但锚定算法保留可参考）

### v1.9.0 (2026-05-12)

**桌面端笔记对比/合并 + 同步重构收官 + 翡翠白瓷主题**

> 📱 移动端（Android）的适配工作也在此版周期完成，但 Android 走**独立版本线**，
> 首个 Android 版以 **0.1.0** 单独发布（见下方「[移动端（Android）](#移动端android)」），
> v1.9.0 本身只含桌面三平台安装包。

桌面端：
- **笔记对比 / 合并**：IDEA 式双栏 diff —— 工具栏「对比剪贴板」、右键「与另一篇笔记对比」、批量栏「对比」、同步冲突手动合并；支持同步滚动开关、合并方向切换、按内容自动选 Markdown 源码 / 纯文本
- **笔记导出 PDF**（走系统打印对话框）+ `Ctrl+Alt+E` 快速导出 Markdown
- **同步重构收官**：UI 区分「多端实时同步 / 快照归档」；WebDAV 附件同步、孤儿附件 GC（宽限期标记 + 清理）、快照 V0 端到端加密、「后台同步」按钮（点了立刻返回，先拉后推后台跑）；推送分小批 + 自适应并发，进度条不再卡 0%
- **任务分类支持拖拽排序**
- **新增「翡翠白瓷」主题**（三栏浅灰 + 翡翠绿点缀）
- **Ollama 支持 function calling**（解锁 MCP / Skills），并修复多个 Ollama 连接 / 系统代理相关问题
- AI 多会话流式不再串台
- 修复：编辑器空行二次保存不再丢失、AI 写作处理选区不再丢图片、若干 UI 细节

### v1.8.1 (2026-05-07)

**文件夹自定义颜色 + 编辑器查找替换**

- **文件夹自定义图标颜色**：右键文件夹 → 弹出 20 色预设色板（与标签同款），一键设色或清除恢复默认主题色；树渲染按 color 实时上色，子文件夹保留中心小白点的层级标识
- **编辑器内查找替换（Ctrl+F / Ctrl+H）**：自实现 Tiptap 3 兼容扩展（社区包仅支 Tiptap 2），浮条 UI 含大小写 / 全词 / 上一个 / 下一个 / 替换 / 全部替换 + n/m 计数；Enter 跳下一、Shift+Enter 跳上一、Esc 关闭

### v1.8.0 (2026-05-04)

**编辑体验稳定性升级 + 外链图片自动落库 + 老 WebView 兼容兜底 + 关于页联系作者**

桌面端 BUG 修复（重要）：
- **拦截 F5 / Ctrl+R 防误刷新丢草稿**：桌面应用不是浏览器，刷新会丢编辑器状态、Zustand 内存状态和未落库草稿。F5 单键尤其容易误触；在 main.tsx capture 阶段拦截 F5 / Ctrl+R / Ctrl+Shift+R / Ctrl+F5
- **笔记编辑页 Ctrl+A 只选笔记内容**：焦点未落到正文时按 Ctrl+A 不再触发 WebView 原生「选中整个文档」把侧栏 / 工具栏 / 元属性都选上，转发到 editor.commands.selectAll
- **粘贴外链图片走 Rust reqwest 绕开防盗链**：从公众号 / 微博 / 知乎复制带图文章，外链图片自动用 reqwest 下载落库（带 Referer 兜底），告别破图
- **修保存内容比当前显示少一字的闭包陷阱**：NoteEditor / TaskDetail / QuickCapture 三处闭包陷阱，快速打字 → 立即返回保存路径会丢最后一字
- **老 WebView 的 lookbehind 不兼容崩溃**：旧版 Edge / WebView2 不支持 RegExp lookbehind 语法，编辑器初始化加 try/catch 兜底，避免直接白屏
- **MCP 容忍 LLM 把 id 传成字符串**：部分模型把 note id 误传成字符串导致 get_note 报错，加自动数字转换

桌面端 UX 增强：
- **关于页加作者介绍 + QQ/微信联系方式**：社区栏头部加作者标签（Java 全栈 AI 架构师 / Agent 架构师）；末尾 Descriptions 加联系作者 QQ/微信号（点击复制，提示备注「来自知识库」），方便从应用内直接联系作者反馈
- **编辑器布局改 absolute+inset**：字数统计挪到元属性栏，长文档滚动 / 工具栏吸顶更稳
- **子文件夹图标加中心小白点**：视觉区分层级，一眼能看出哪些文件夹是子级
- **NewNoteButton 下拉菜单补齐「导入 Markdown 文件夹」**：之前只能导入单文件，现在与右键菜单入口对齐

移动端（Android）—— 开发中预览，本版桌面端发布不含 APK：
- Tauri Android target 接入 + 真机跑通
- 大量 cfg gate 隔离桌面专属代码（rust-s3 / pdfium / calamine / docx-rs / autostart 等）
- 移动版页面：笔记编辑 / 全文搜索 / 标签 / 回收站 / AI 聊天 / 任务详情 / 闪念捕获 / 我的 Tab / Dashboard 30 天写作热力图 / 闪卡复习 / 模型管理 / WebDAV 手动推拉 / 单文件导入 / 网页剪藏 / 桌面专属设置项隔离
- 主页 Dashboard 与底部 Tab 4 格可定制
- Android debug APK CI 工作流（暂不进 update.json，仅供内测）
- 移动端 QA 核查清单 120+ 项跟踪中

### v1.7.1 (2026-05-03)

**语音录入体验优化 + 导入目录行为调整 + 编辑器大纲优化**

新增 / 优化：
- **语音录入快捷键体验优化**：全局快捷键降级为应用内快捷键，移除 `Ctrl+Shift+V`，减少与系统和其他软件快捷键冲突
- **麦克风状态同步更清晰**：MicButton 镜像全局录音状态，工具条录音时同步变红，录音反馈更一致
- **导入预览默认更贴近日常使用**：默认直接导入源目录内部内容，不额外包一层源根目录；仍可勾选「保留原目录文件夹」按旧方式导入
- **编辑器大纲可隐藏**：右侧大纲增加隐藏入口，减少编辑时的常态视觉干扰
- **大纲宽度可快速还原**：双击大纲分隔条恢复默认宽度，拖拽宽度继续持久化保存

UI / 交互：
- 大纲拖拽分隔条改为仅用光标提示，移除 hover 蓝色背景，编辑区视觉更安静
- 清理侧栏与笔记面板导入入口的冗余默认参数，统一导入预览行为

### v1.7.0 (2026-05-02)

**Linux 启动 BUG 修复 + 中文资产路径加载修复 + 录音音量波形 + 多输入框语音**

修复：
- **Linux 单实例锁残留导致打不开窗口（高优先级）**：旧实现用 `create_new(true)` 当锁，进程退出锁文件不删，下次启动永远被误判成"已运行"——要么 ping 默认实例后直接退出（用户看不到窗口），要么不停往后分配 instance-2/3/.../99 形成 zombie 实例。改为 Unix 一律走 `flock(LOCK_EX|LOCK_NB)`，内核在 fd 关闭时自动释放（含 panic / SIGKILL），锁文件即使残留也不再误判
- **中文资产文件名加载失败**：tiptap-markdown 序列化会把 `![](kb-asset://中文.png)` 编码成 `%E4%B8%AD%E6%96%87.png` 写入 .md，重新加载后按字面值在磁盘里找文件直接 404。`parseKbAsset` 增加 `decodeURIComponent`，失败回退原值避免崩溃
- **输入框内点麦克风光标消失**：Button mousedown 默认抢焦点导致 Input 失焦光标停闪。加 `onMouseDown preventDefault` 阻断焦点转移，转写完拼接文本无需重新聚焦

新增 / 优化：
- **录音按钮加实时音量波形**：新增 `useAudioLevel` Hook 通过 Web Audio AnalyserNode 60fps 采集分频段强度；MicButton 录音时把 Square 图标换成 3 条柱实时跳动 + 按音量放大红环 box-shadow（脉动反馈），QuickCaptureAsrModal 同款放大版
- **更多输入框接入语音**：把 MicButton 铺到笔记标题 / 笔记编辑器标题 / 笔记列表搜索 / 任务搜索 / Prompt 标题与说明 / 标签筛选 / 全局搜索 / 日记标题等多处入口
- **AI 对话页麦克风按钮位置调整**：从 TextArea 前换到后，避免和发送键挤一起
- **输入框统一 allowClear**：HomeSearchInput / DraftNoteModal 标题 / CommandPalette 输入都补带 X 按钮一键清空
- **Rust 全量 cargo fmt 格式化**：跨 commands / database / services / kb-core / mcp / tray，零业务逻辑变化

### v1.6.0 (2026-04-30)

**思维导图视图 + pop-out 多窗口分屏 + 任务子步骤 + 任务详情 Modal + 赞赏页**

新增功能：
- **思维导图视图（基于 markmap）**：笔记编辑器顶栏 🧠 按钮把 markdown 标题层级实时渲染为思维导图，与编辑器并排嵌入式分栏（真分屏，不是浮层）；可拖拽 splitter 调宽（320–1200px，宽度持久化），打开时大纲自动隐藏；支持放大/缩小/自适应/导出 SVG（经 Tauri save 对话框 + Rust 写盘）
- **笔记 pop-out 多窗口**：编辑器顶栏 ↗ 按钮把当前笔记弹到独立 OS 窗口（精简模式无侧栏/Tabs），用于双显示器对照、两笔记对比；同 note_id 已存在窗口直接前置
- **任务子步骤（参考 MS To Do 模型）**：单层不嵌套；列表行 ▶ 行内展开勾选；进度徽章 `done/total` 实时更新（局部 patch 不刷新整列表）；主任务完成不强制勾选子任务（与业界主流一致）
- **任务详情 Modal（只读）**：点任务行不再直接进编辑态，改为弹只读详情 Modal（与首页今日待办一致），含子任务区；左下「编辑」按钮 / hover 铅笔图标 / 右键菜单"编辑任务"才进编辑 Modal；首页今日待办卡片定高 280px + 内部滚动
- **赞赏支持页**：关于页加「赞赏支持」锚点 + 微信赞赏码；「作者 & 社区」加 QQ 交流群（群号 1090770702）；文档站新建 sponsor.md 收纳所有支持方式

修复 / 优化：
- 笔记列表 / 侧边栏树拖动排序（含拖到顶部 / 闪烁优化）
- .md 导入保留行内 / fence 外缩进、段内单换行渲染为 `<br>`
- 图文混合粘贴本地化（含 Office Word file:// 临时图）
- 截图粘贴去重（`items` 优先 + `files` 兜底）

### v1.5.0 (2026-04-28)

**待办分类、全局搜索覆盖待办、首页搜索 dropdown、搜索高亮精准化**

新增功能：
- **待办一级分类**（`task_categories` 表 + `tasks.category_id` 外键）：彩色圆点 + 自定义名称 + 排序，支持「未分类」虚拟节点；侧栏 TasksPanel 加分类 section（点击筛选 + 计数徽章 + 管理弹窗）；任务行内显示分类色点 + 名字；新建/编辑表单含分类下拉
- **全局搜索覆盖待办**：新增 `search_tasks` Command（按 title/description LIKE，按状态/优先级/截止日排序）；Ctrl+K 命令面板加待办分组 + 命中跳 `/tasks?taskId=N` 自动开编辑 Modal
- **首页搜索 dropdown**：输入即并发拉笔记 + 待办（200ms 防抖），点击直跳详情；回车保留原行为去 `/search?q=` 看完整结果
- **`/search` 全量结果页**：加 `Segmented` Tab「全部 / 笔记 / 待办」+ URL `?type=` 持久化；笔记 Tab 保留虚拟滚动；待办点击跳编辑 Modal
- **跳到笔记自动展开侧栏文件树**：进入 `/notes/:id` 时 NotesPanel 自动收集祖先文件夹路径并展开，高亮目标笔记

搜索体验升级：
- **标题字符级高亮**：用户搜「本地」只高亮「本地」两字，不会因 unicode61 长 token 把整个 token 段（如「本地优先的知识库桌面应用」）一整块染色
- **标题命中优先排序**：FTS5 路径 `bm25(notes_fts, 5.0, 1.0)` 让 title 权重 5×；LIKE fallback 加 `_title_score` 计算列让标题命中排前
- **召回率提升**：FTS5 改 prefix 查询 `本地*`，解决 unicode61 中文长 token 漏命中（搜「本地」能命中「本地仓库说明」标题）
- **snippet 双行 line-clamp**：避免单行 truncate 把高亮关键词推到右边截掉
- **FTS5 路径补 is_hidden 过滤**：与 LIKE fallback 行为一致，隐藏笔记不再泄露到主搜索结果
- **顶栏命令面板加「待办」页面跳转**：Ctrl+K 输入"待办"可直跳 `/tasks`

BUG 修复：
- `get_task` SELECT 漏 `category_id` 列导致命中"任务不存在"误报；`.ok()` 静默吞错改为 `.optional()?` 让真实 SQL 错误暴露
- 创建分类时 UNIQUE 冲突错误从「数据库错误: UNIQUE constraint failed」改为友好的「分类名称「xxx」已存在」+ 前端预校验 disable 按钮

UI：
- "添加待办"按钮统一改主题色（与"新建笔记"对齐）

### v1.4.0 (2026-04-27)

**编辑器工具栏全面升级 + 多项 BUG 修复**

新增编辑器功能：
- **段落格式 H1–H6 下拉**（替代旧 H1/H2/H3 三按钮，支持完整六级标题，Notion 风视觉层级）
- **字体颜色 / 背景颜色** ColorPicker（12 色预设）
- **字号下拉**（11 档 12px–48px）+ **行间距下拉**（6 档 1.0–2.0）
- **上标 / 下标** 按钮（公式/化学式场景）
- **段落缩进 / 减少缩进** 按钮
- **清除格式** 按钮
- **Callout 提示框**（4 种类型：信息 / 提示 / 警告 / 危险，块内 emoji 切类型）
- **Toggle 折叠块**（▶ 标题 + 可折叠多行内容，标准 HTML `<details>` 兼容）
- **字数统计**（工具栏右浮，hover 弹层看字数 / 字符 / 段落 / 阅读时长）
- **Emoji 选择器**（10 类约 280 个常用 emoji，水平分类条 + 网格）
- **工具栏插入视频 / 视频时间戳 / 附件** 按钮(与"插入图片"对称)

孤儿素材清理升级：
- 一次扫描覆盖 5 类素材（images / videos / attachments / pdfs / sources），按类别分组展示
- 修复旧版"撤回笔记图片消失"和"加密笔记图片误删"两个 BUG

更新策略 & 兜底：
- updater 端点 R2 → Gitee → GitHub 三级容错（Gitee 优先国内体验更稳）
- 自动更新失败时显示"手动下载页"链接（Gitee / GitHub Releases）

BUG 修复：
- 浏览器复制图片粘贴 → 自动保存到本地（之前显示破图）
- 附件 / 链接点击用系统应用打开（不再跳浏览器；解决 Tauri WebView anchor navigation 旁路问题）
- markdown-it `validateLink` 放行 `file://` 协议（之前二次打开附件链接降级成纯文本）
- Link mark 渲染 / 解析双向支持 `<span data-href>` + `<a href>`（避免序列化往返破坏）
- 行内代码 `<code>` 不再排斥其他 mark，可同时设字号 / 颜色 / 高亮
- 默认字号 / 行高 unset 命令链恢复（之前点击无反应）
- 工具栏紧凑化（删两端对齐 + 独立移除链接按钮，按钮间距/divider 微调）

### v1.3.1 (2026-04-27)

**Windows 同步恢复 1224 错误紧急修复**

- 修复 WebDAV 从云端拉取 / 从本地 ZIP 导入时报 "IO 错误：请求的操作无法在使用用户映射区域打开的文件上执行 (os error 1224)" 的问题
- 根因：SQLite 默认 mmap 持有 `app.db` 文件，apply 阶段 `fs::File::create(app.db)` 撞上 Windows 的 `ERROR_USER_MAPPED_FILE`
- 解决：Database 新增 `release()` 方法，在 apply 前临时把 connection 切到 `:memory:` 释放 mmap，apply 完无论成败都 reopen 回真实路径
- 顺手修：拉取/导入完成后 "数据已重载" toast 重复出现两次（前端 `useEffect` cleanup 在 React 严格模式下踩到 async listen 注册竞态，导致 listener 泄漏）

### v1.3.0 (2026-04-26)

**多端同步 V1 自动调度（双向）**
- 新增 `sync_v1_scheduler` 后台任务：每分钟扫一遍 enabled+auto_sync 的 backend，到期跑 `pull → push` 双向同步（git workflow 习惯）
- pull 失败跳过 push 避免把过期数据推回去；失败 `emit sync_v1:auto-triggered` 让前端弹 toast
- SyncV1Section "自动同步" Switch 解禁（之前是「待实现」占位）
- 仅默认实例启动 scheduler，避免多实例对同一远端竞速覆盖

**关闭按钮行为可配置**
- 设置 → 启动设置 新增「关闭窗口时」三选一：每次询问 / 最小化到托盘 / 直接退出
- 默认「每次询问」：弹出三按钮 Modal（最小化 / 退出 / 取消 X）+ Checkbox「记住我的选择」自动写回配置
- "直接退出"分支转发 `tray:request-exit`，复用 `ExitConfirmListener` 的脏数据保存检查
- on_window_event 加主窗判断，子窗口（紧急提醒 / 迁移 splash）的关闭走系统默认不被拦截

**允许多开实例开关**
- 设置 → 启动设置 新增「允许多开实例」开关；默认禁止，再次启动会唤起已有窗口
- 后端 flag 文件存于 framework_app_data_dir，启动早期一次 `Path::exists()` 决策
- `--instance N` 命令行仍可绕开（开发/调试逃生口）

**新建笔记支持 TXT 导入**
- 下拉菜单新增「导入 Markdown / TXT…」，dialog filter 一次接受 `[md, markdown, txt]` 可批量混选（业界 VS Code / Bear / Apple Notes 同质合并模式）
- 后端 `read_text_auto_encoding` 工具：UTF-8 优先 → 失败用 chardetng 嗅探（覆盖 GBK / GB18030 / Big5 / Shift_JIS 等老 .txt）→ encoding_rs 解码
- 命令行 / 双击 .txt 也能投递到本应用（`extract_md_paths_from_args` 加 .txt）

**导入后智能跳转**
- 后端 `ImportResult` 新增 `noteIds` / `existingNoteIds`：分别记录新建 ID 和被 Skip 命中的现有 ID
- 前端按总数分流：1 篇直开编辑器 / 0 新+1 命中已有也跳那篇（"我导入是为了打开它"）/ 多篇跳列表
- PDF / Word 流程同样套用，行为统一

**同步源管理 UI 优化**
- "新增 backend" → "新增同步源"全面中文化（含表格空态、删除确认、Modal 标题）
- 弹窗加固定高度 + 内部滚动，S3 字段多也不会撑出视口
- WebDAV 子表单加「复用「备份与恢复」配置」按钮，从 V0 解密密码自动填充

**布局微调**
- 折叠按钮按需渲染（首页/设置/关于这类无 SidePanel 的页面整体不显示）
- 后退/前进按钮接 React Router history.state.idx 智能禁用，刚启动时两按钮 disabled
- 折叠按钮图标换 lucide PanelLeft 系列（与 VS Code / Cursor 一致）
- ActivityBar 加 height: 100% 修复底部三项（隐藏笔记 / 回收站 / 关于）未钉到左下角的 bug

**笔记列表样式微调**
- 序号列 width 36 + cell padding 4，更紧凑
- 标题列 paddingRight 24，与右侧"目录"列拉开呼吸空间

**编辑器 / 紧急提醒**
- WikiLink 全角双括号 【【 自动转半角 [[
- 紧急待办独立提醒窗口流程优化（关闭决策 + 重弹机制）

### v1.2.0 (2026-04-24)

**侧边栏大改造（方案 C：Activity Bar 模式）**
- 主侧栏拆成左 48px 图标栏 + 右 240px 视图面板，仿 VS Code / Obsidian
- 笔记 / 标签 / 每日 / 搜索 / 待办 5 个视图统一"左选分组 + 右看内容"范式
- URL 驱动：每个筛选条件都能直达、可书签（`/tasks?filter=overdue`、`/tags?tagId=5` 等）
- SidePanel 宽度与可见性持久化；`Ctrl/⌘` + 点当前图标 = 折叠/展开面板（VS Code 行为）

**待办视图重写**
- 11 维度 4 分组的智能列表：进行中 / 逾期 / 今天 / 本周 / 无日期 / 紧急 / 普通 / 低 / 循环 / 有关联 / 已完成
- Badge 数字实时从后端 stats 派生；主区标题动态跟随选中项（"今天的任务" / "逾期任务" …）
- AI 规划今日：基于今日笔记 + 待办产出 3~7 条行动建议（T-005）

**笔记列表批量操作**
- 多选复选框 + 浮出工具条：批量移动文件夹、批量打标签（不清除原有）、批量移到回收站
- 一次 IPC + 一条 SQL，原子性 + 性能；不改 `updated_at` 避免冒到最前

**AI Skills 工具框架（T-004）**
- 模型可调 5 个只读工具（读取笔记 / 搜索 / 列文件夹 / 列标签 / 统计）
- tool_calls 解析 + dispatch，状态机 ok/error/running 规范化

**隐藏与加密（T-003 / T-007）**
- 笔记"隐藏"标记：主列表/搜索隐藏但能从 wiki 链接打开；独立"隐藏笔记"视图
- Vault：主密码保护整个库，支持一键加密/解密单条笔记

**编辑器 & 产物**
- 附件拖拽落库；文本 / Markdown / 图片分支路径
- 长文档工具栏吸顶（修 `.tiptap-wrapper` 的 overflow:hidden 打断 sticky）
- 每日笔记：URL `?date=` 驱动 + 拖图按需建档 + SidePanel 日期列表
- 图片保存文件名加进程内原子计数器，避免同毫秒覆盖

**PDF 原文件预览**
- 标题栏加"最大化"和"用系统应用打开"按钮
- 老旧 WebView2 拦截 asset 协议时用户仍能一键切系统阅读器

**Bug 修复**
- AI 提示词编辑内置模板时表单全空（Form 懒挂载 + setFieldsValue 时序坑）
- 虚拟列表 `contain: strict` 致容器 0 高度（tags 笔记列表）
- image.rs 同毫秒文件名冲突
- 笔记列表分页条与行白底割裂 / 表格行分隔线冗余 / 加绝对索引列

### v1.1.0 (2026-04-22)

**性能优化（大知识库下尤为明显）**
- SQLite 补齐 PRAGMA：`synchronous=NORMAL` + `cache_size=64MB` + `mmap_size=32MB` + `temp_store=MEMORY` + `busy_timeout=5s`
- 新增索引 `idx_note_links_source` — 保存笔记时删除旧链接的 DELETE 走索引，10k 笔记下保存速度大幅提升
- 新增 `notes.title_normalized` 列 + 部分索引（v17 迁移，Rust 侧批量回填）— wiki 链接匹配从全表扫 O(n) 降到索引 O(log n)
- `get_dashboard_stats` 6 次 query_row 合并为 2 条 SQL + `substr` 替换 `LIKE` 前缀匹配
- `get_graph_data` 相关子查询改为 `LEFT JOIN ... GROUP BY`，消除 N+1
- `reqwest::Client` 用 `OnceLock` 做进程级单例（AI 流式 / WebDAV 都复用），消除每次请求重建连接池和 TLS 会话的开销
- 孤儿图片扫描去除 GB 级 haystack 字符串拼接，改用手写状态机 + `HashSet`，大库下内存占用和扫描时间都显著下降
- `export_notes` 把数据读取和文件写入分离，文件 I/O 前主动释放 DB 锁，导出期间不再阻塞其他 Command

**前端渲染**
- TipTap 编辑器 `onUpdate` 加 300ms 防抖 + `onBlur` / unmount 强制 flush，长笔记打字不再卡顿
- 搜索结果页、标签笔记列表接入 `@tanstack/react-virtual` 虚拟滚动，千级结果也不卡

**UI / 交互**
- 笔记编辑页目录切换升级为"面包屑 + Popover Tree 选择"（Notion / Obsidian 风格）
- 笔记列表"目录"列改成可点击修改所属文件夹（就地 Popover，保留"筛选此文件夹"快捷入口）
- 侧边栏文件夹区：顶部间距 + 箭头右移 + "再次点击已选文件夹 → 取消筛选回到全部笔记"
- 修复文件夹右键菜单点击外部不关闭的问题（补全 mousedown / Esc 全局监听）
- 设置页 & 关于页新增"作者 & 社区"卡片（B 站主页 + 知识星球）

### v1.0.0 (2026-04-21)

**新增**
- 系统托盘右键菜单大扩展：新建笔记（Ctrl+N）/ 打开今日每日笔记 / 全局搜索（Ctrl+K）/ 立即同步到云端 / 窗口置顶 / 开机自启 / 检查更新
- 右上角标题栏新增"窗口置顶"图钉按钮（PushpinOutlined / PushpinFilled），与托盘 CheckMenuItem 双向同步
- 窗口置顶状态跨应用重启持久化（tauri-plugin-store）
- 文档站下载页改造：构建时从 R2 `versions.json` 注入版本列表（消除运行时 GitHub API 限流 + CORS 问题）

**修复**
- 修复托盘左键再次点击不能最小化回托盘（改为 toggle 可见性）
- 补全 Capabilities 权限 `core:window:allow-set-always-on-top`，置顶功能实际生效

**改进**
- 托盘"立即同步"复用 `sync_scheduler::push_once`，新独立事件 `sync:manual-push-result` 避免与设置页 toast 冲突
- release-publish skill 文档扩展：新增"更新 R2 versions.json" + "触发文档站重建"两步

### v0.2.0 (2026-04-19)

**新增**
- WebDAV 多设备云同步（本地 ZIP 快照 + WebDAV 全量推送 + 定时自动同步 + "从其他设备拉取"一键恢复）
- AES-256-GCM 加密存储 WebDAV 密码（跨机器安全，无需 keyring）
- 模板编辑升级为 Tiptap 富文本，弹窗扩大至 820px
- 全局"新建笔记"入口（Modal 整合空白/导入 md/pdf/Word/模板 5 种来源）
- 设置页直接入口：PDF / Word 批量导入（WPS Office / COM 多 ProgId 兜底 + 诊断面板）
- "检查更新"按钮 + 官网跳转（kb.ruoyi.plus）
- 知识图谱连线功能完整可用（wiki 链接精确匹配 + 自动写入 note_links）

**改进**
- 知识图谱布局参数按 G6 v5 子对象 API 重写（link / manyBody / collide），节点布局更合理
- 知识图谱 autoFit 使用 `when: "overflow"`，装得下保原尺寸字不被缩小
- 笔记视图切换卡顿优化
- 同步历史从 JSON 原文改为人类可读摘要（方向 / 条数 / 大小）
- 应用名统一为"知识库"（原"本地知识库"降级为卖点描述）
- antd message / notification 迁到 `App.useApp()` 上下文版，提示稳定显示

**修复**
- Wiki 链接保存时采用规范化精确匹配（trim + 空白折叠 + 大小写不敏感），失败时 notification 列出所有未命中标题
- Tiptap StarterKit 与手动 Link/Underline 扩展重复的警告
- Tabs 栏在删除 / 清空回收站时残留
- 清空回收站同步删除关联 PDF / Word / 图片资源（启用 SQLite `foreign_keys = ON`）

### v0.1.1 (2026-04-16)

- 接入 Cloudflare R2 CDN 作为主更新端点（国内下载提速 10-100 倍）
- updater 端点双容错：R2 优先，GitHub raw 兜底
- 修复：首次 CI 构建失败的遗留问题（Cargo.toml panic 策略 + unused import）

### v0.1.0 (2026-04-16)

- 首次发布
- Markdown 编辑（Tiptap）、全文搜索、双向链接、知识图谱、AI 问答
- 新极简字母 K 图标
- 支持 Windows x64 / macOS (ARM + Intel)

## 项目结构

```
releases/
├── v0.1.0/
│   └── ...
├── v0.1.1/
│   └── ...
├── v0.2.0/
│   └── ...
├── v1.0.0/
│   └── ...
├── v1.1.0/
│   └── ...
├── v1.2.0/
│   └── ...
├── v1.3.0/
│   └── ...
├── v1.3.1/
│   └── ...
├── v1.4.0/
│   └── ...
├── v1.5.0/
│   └── ...
├── v1.6.0/
│   └── ...
├── v1.7.0/
│   └── ...
├── v1.7.1/
│   └── ...
├── v1.8.0/
│   └── ...
├── v1.8.1/
│   └── ...
├── v1.9.0/
│   └── ...
├── v1.10.0/
│   └── ...
├── v1.11.0/
│   └── ...
├── v1.12.0/
│   └── ...
├── v1.13.0/
│   └── ...
├── v1.14.0/
│   └── ...
├── ...                                             # v1.20.0 – v1.32.0 同构，略
└── v1.50.0/
    ├── Knowledge.Base_1.50.0_x64-setup.exe         # Windows 安装包
    ├── Knowledge.Base_1.50.0_x64-setup.exe.sig     # Windows 签名
    ├── Knowledge.Base_1.50.0_x64-setup.nsis.zip    # Windows updater 压缩包
    ├── Knowledge.Base_1.50.0_aarch64.dmg           # macOS ARM 安装镜像
    ├── Knowledge.Base_1.50.0_x64.dmg               # macOS Intel 安装镜像
    ├── Knowledge.Base_aarch64.app.tar.gz              # macOS ARM updater
    ├── Knowledge.Base_aarch64.app.tar.gz.sig          # macOS ARM updater 签名
    ├── Knowledge.Base_x64.app.tar.gz                  # macOS Intel updater
    ├── Knowledge.Base_x64.app.tar.gz.sig              # macOS Intel updater 签名
    ├── Knowledge.Base_1.50.0_amd64.deb             # Linux Debian/Ubuntu 包
    ├── Knowledge.Base_1.50.0_amd64.AppImage        # Linux 通用 AppImage（>100MB，仅 R2）
    ├── Knowledge.Base_1.50.0_amd64.AppImage.tar.gz # Linux updater 压缩包（仅 R2）
    └── Knowledge.Base_1.50.0_amd64.AppImage.tar.gz.sig # Linux updater 签名

update.json                                         # 自动更新元数据（GitHub 版）
update-r2.json                                      # 自动更新元数据（R2 版，备档）
```
