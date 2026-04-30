# Knowledge Base - Release

本地优先的知识库桌面应用（Tauri 2.x + React 19）的安装包与自动更新端点仓库。

## 最新版本: v1.6.0

| 平台 | 下载链接 |
|------|---------|
| Windows x64 | [Knowledge.Base_1.6.0_x64-setup.exe](releases/v1.6.0/Knowledge.Base_1.6.0_x64-setup.exe) |
| macOS Apple Silicon | [Knowledge.Base_1.6.0_aarch64.dmg](releases/v1.6.0/Knowledge.Base_1.6.0_aarch64.dmg) |
| macOS Intel | [Knowledge.Base_1.6.0_x64.dmg](releases/v1.6.0/Knowledge.Base_1.6.0_x64.dmg) |
| Linux x64 (deb) | [Knowledge.Base_1.6.0_amd64.deb](releases/v1.6.0/Knowledge.Base_1.6.0_amd64.deb) |
| Linux x64 (AppImage) | [Knowledge.Base_1.6.0_amd64.AppImage](releases/v1.6.0/Knowledge.Base_1.6.0_amd64.AppImage) |

## 自动更新

应用内自动更新走双端点容错：

| 优先级 | 端点 | 说明 |
|-------|------|------|
| 1 (主) | `https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/update.json` | Cloudflare R2 CDN，国内快 |
| 2 (备) | `https://github.com/bkywksj/knowledge-base-release/raw/main/update.json` | GitHub raw 兜底 |

## 版本历史

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
- **工具栏插入视频 / 视频时间戳 / 附件** 按钮（与"插入图片"对称）

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
└── v1.6.0/
    ├── Knowledge.Base_1.6.0_x64-setup.exe         # Windows 安装包
    ├── Knowledge.Base_1.6.0_x64-setup.exe.sig     # Windows 签名
    ├── Knowledge.Base_1.6.0_x64-setup.nsis.zip    # Windows updater 压缩包
    ├── Knowledge.Base_1.6.0_aarch64.dmg           # macOS ARM 安装镜像
    ├── Knowledge.Base_1.6.0_x64.dmg               # macOS Intel 安装镜像
    ├── Knowledge.Base_aarch64.app.tar.gz          # macOS ARM updater
    ├── Knowledge.Base_aarch64.app.tar.gz.sig      # macOS ARM updater 签名
    ├── Knowledge.Base_x64.app.tar.gz              # macOS Intel updater
    ├── Knowledge.Base_x64.app.tar.gz.sig          # macOS Intel updater 签名
    ├── Knowledge.Base_1.6.0_amd64.deb             # Linux Debian/Ubuntu 包
    ├── Knowledge.Base_1.6.0_amd64.AppImage        # Linux 通用 AppImage
    ├── Knowledge.Base_1.6.0_amd64.AppImage.tar.gz # Linux updater
    └── Knowledge.Base_1.6.0_amd64.AppImage.tar.gz.sig # Linux updater 签名
update.json                                         # 自动更新元数据（GitHub 版）
update-r2.json                                      # 自动更新元数据（R2 版，备档）
```
