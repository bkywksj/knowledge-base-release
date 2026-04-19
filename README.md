# Knowledge Base - Release

本地优先的知识库桌面应用（Tauri 2.x + React 19）的安装包与自动更新端点仓库。

## 最新版本: v0.2.0

| 平台 | 下载链接 |
|------|---------|
| Windows x64 | [Knowledge.Base_0.2.0_x64-setup.exe](releases/v0.2.0/Knowledge.Base_0.2.0_x64-setup.exe) |
| macOS Apple Silicon | [Knowledge.Base_0.2.0_aarch64.dmg](releases/v0.2.0/Knowledge.Base_0.2.0_aarch64.dmg) |
| macOS Intel | [Knowledge.Base_0.2.0_x64.dmg](releases/v0.2.0/Knowledge.Base_0.2.0_x64.dmg) |

## 自动更新

应用内自动更新走双端点容错：

| 优先级 | 端点 | 说明 |
|-------|------|------|
| 1 (主) | `https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/update.json` | Cloudflare R2 CDN，国内快 |
| 2 (备) | `https://github.com/bkywksj/knowledge-base-release/raw/main/update.json` | GitHub raw 兜底 |

## 版本历史

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
└── v0.2.0/
    ├── Knowledge.Base_0.2.0_x64-setup.exe         # Windows 安装包
    ├── Knowledge.Base_0.2.0_x64-setup.exe.sig     # Windows 签名
    ├── Knowledge.Base_0.2.0_x64-setup.nsis.zip    # Windows updater 压缩包
    ├── Knowledge.Base_0.2.0_aarch64.dmg           # macOS ARM 安装镜像
    ├── Knowledge.Base_0.2.0_x64.dmg               # macOS Intel 安装镜像
    ├── Knowledge.Base_aarch64.app.tar.gz          # macOS ARM updater
    ├── Knowledge.Base_aarch64.app.tar.gz.sig      # macOS ARM updater 签名
    ├── Knowledge.Base_x64.app.tar.gz              # macOS Intel updater
    └── Knowledge.Base_x64.app.tar.gz.sig          # macOS Intel updater 签名
update.json                                         # 自动更新元数据（GitHub 版）
update-r2.json                                      # 自动更新元数据（R2 版，备档）
```
