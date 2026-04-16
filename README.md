# Knowledge Base - Release

本地优先的知识库桌面应用（Tauri 2.x + React 19）的安装包与自动更新端点仓库。

## 最新版本: v0.1.0

| 平台 | 下载链接 |
|------|---------|
| Windows x64 | [Knowledge.Base_0.1.0_x64-setup.exe](releases/v0.1.0/Knowledge.Base_0.1.0_x64-setup.exe) |
| macOS Apple Silicon | [Knowledge.Base_0.1.0_aarch64.dmg](releases/v0.1.0/Knowledge.Base_0.1.0_aarch64.dmg) |
| macOS Intel | [Knowledge.Base_0.1.0_x64.dmg](releases/v0.1.0/Knowledge.Base_0.1.0_x64.dmg) |

## 自动更新

应用内自动更新通过本仓库的 `update.json` 提供：

```
https://github.com/bkywksj/knowledge-base-release/raw/main/update.json
```

## 版本历史

### v0.1.0 (2026-04-16)

- 首次发布
- Markdown 编辑（Tiptap）、全文搜索、双向链接、知识图谱、AI 问答
- 新极简字母 K 图标
- 支持 Windows x64 / macOS (ARM + Intel)

## 项目结构

```
releases/
└── v0.1.0/
    ├── Knowledge.Base_0.1.0_x64-setup.exe         # Windows 安装包
    ├── Knowledge.Base_0.1.0_x64-setup.exe.sig     # Windows 签名
    ├── Knowledge.Base_0.1.0_x64-setup.nsis.zip    # Windows updater 压缩包
    ├── Knowledge.Base_0.1.0_aarch64.dmg           # macOS ARM 安装镜像
    ├── Knowledge.Base_0.1.0_x64.dmg               # macOS Intel 安装镜像
    ├── Knowledge.Base_aarch64.app.tar.gz          # macOS ARM updater
    ├── Knowledge.Base_aarch64.app.tar.gz.sig      # macOS ARM updater 签名
    ├── Knowledge.Base_x64.app.tar.gz              # macOS Intel updater
    └── Knowledge.Base_x64.app.tar.gz.sig          # macOS Intel updater 签名
update.json                                         # 自动更新元数据
```
