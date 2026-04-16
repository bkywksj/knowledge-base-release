# Knowledge Base - Release

本地优先的知识库桌面应用（Tauri 2.x + React 19）的安装包与自动更新端点仓库。

## 最新版本: v0.1.1

| 平台 | 下载链接 |
|------|---------|
| Windows x64 | [Knowledge.Base_0.1.1_x64-setup.exe](releases/v0.1.1/Knowledge.Base_0.1.1_x64-setup.exe) |
| macOS Apple Silicon | [Knowledge.Base_0.1.1_aarch64.dmg](releases/v0.1.1/Knowledge.Base_0.1.1_aarch64.dmg) |
| macOS Intel | [Knowledge.Base_0.1.1_x64.dmg](releases/v0.1.1/Knowledge.Base_0.1.1_x64.dmg) |

## 自动更新

应用内自动更新走双端点容错：

| 优先级 | 端点 | 说明 |
|-------|------|------|
| 1 (主) | `https://pub-9d9e6c0cb6934fb0a0c505e3c64f39b2.r2.dev/knowledge-base/update.json` | Cloudflare R2 CDN，国内快 |
| 2 (备) | `https://github.com/bkywksj/knowledge-base-release/raw/main/update.json` | GitHub raw 兜底 |

## 版本历史

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
└── v0.1.1/
    ├── Knowledge.Base_0.1.1_x64-setup.exe         # Windows 安装包
    ├── Knowledge.Base_0.1.1_x64-setup.exe.sig     # Windows 签名
    ├── Knowledge.Base_0.1.1_x64-setup.nsis.zip    # Windows updater 压缩包
    ├── Knowledge.Base_0.1.1_aarch64.dmg           # macOS ARM 安装镜像
    ├── Knowledge.Base_0.1.1_x64.dmg               # macOS Intel 安装镜像
    ├── Knowledge.Base_aarch64.app.tar.gz          # macOS ARM updater
    ├── Knowledge.Base_aarch64.app.tar.gz.sig      # macOS ARM updater 签名
    ├── Knowledge.Base_x64.app.tar.gz              # macOS Intel updater
    └── Knowledge.Base_x64.app.tar.gz.sig          # macOS Intel updater 签名
update.json                                         # 自动更新元数据（GitHub 版）
update-r2.json                                      # 自动更新元数据（R2 版，备档）
```
