# Knowledge Base - Release

本地优先的知识库桌面应用（Tauri 2.x + React 19）的安装包与自动更新端点仓库。

## 最新版本: v1.2.0

| 平台 | 下载链接 |
|------|---------|
| Windows x64 | [Knowledge.Base_1.2.0_x64-setup.exe](releases/v1.2.0/Knowledge.Base_1.2.0_x64-setup.exe) |
| macOS Apple Silicon | [Knowledge.Base_1.2.0_aarch64.dmg](releases/v1.2.0/Knowledge.Base_1.2.0_aarch64.dmg) |
| macOS Intel | [Knowledge.Base_1.2.0_x64.dmg](releases/v1.2.0/Knowledge.Base_1.2.0_x64.dmg) |

## 自动更新

应用内自动更新走双端点容错：

| 优先级 | 端点 | 说明 |
|-------|------|------|
| 1 (主) | `https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/update.json` | Cloudflare R2 CDN，国内快 |
| 2 (备) | `https://github.com/bkywksj/knowledge-base-release/raw/main/update.json` | GitHub raw 兜底 |

## 版本历史

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
└── v1.2.0/
    ├── Knowledge.Base_1.2.0_x64-setup.exe         # Windows 安装包
    ├── Knowledge.Base_1.2.0_x64-setup.exe.sig     # Windows 签名
    ├── Knowledge.Base_1.2.0_x64-setup.nsis.zip    # Windows updater 压缩包
    ├── Knowledge.Base_1.2.0_aarch64.dmg           # macOS ARM 安装镜像
    ├── Knowledge.Base_1.2.0_x64.dmg               # macOS Intel 安装镜像
    ├── Knowledge.Base_aarch64.app.tar.gz          # macOS ARM updater
    ├── Knowledge.Base_aarch64.app.tar.gz.sig      # macOS ARM updater 签名
    ├── Knowledge.Base_x64.app.tar.gz              # macOS Intel updater
    └── Knowledge.Base_x64.app.tar.gz.sig          # macOS Intel updater 签名
update.json                                         # 自动更新元数据（GitHub 版）
update-r2.json                                      # 自动更新元数据（R2 版，备档）
```
