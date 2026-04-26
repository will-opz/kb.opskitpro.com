# OpsKitPro Content Engine (Obsidian Vault)

欢迎使用 OpsKitPro 的内容生产线！这套结构专为 **“工具站 + SEO 内容产出”** 设计。

## 📁 目录说明

1.  **`00_Inbox/`**：随手记录原始故障现象、报错信息或灵感。
2.  **`01_Notes/`**：对 Inbox 中的内容进行初步整理，记录解决逻辑。
3.  **`02_Articles/`**：**SEO 文章产出区**。只有这里的文章才会被发布到网站。使用 `tpl-article` 模板。
4.  **`03_Tools/`**：OpsKitPro 自家工具的详细说明与操作手册。
5.  **`04_Templates/`**：全套 Obsidian 模板，支持快速生成内容。
6.  **`05_Assets/`**：存放截图、数据抓包附件等。

## 🚀 极简工作流

1.  在 **`00_Inbox`** 记录遇到的运维问题。
2.  使用 AI (如 Antigravity 或 ChatGPT) 根据记录生成 SEO 文章。
3.  将生成的内容存入 **`02_Articles`**，检查路径名（全小写、用 `-` 隔开）。
4.  Push 到 GitHub，网站自动根据这些 Markdown 文件更新博客/文档。

## 🔓 对外开源建议

如果要把 `kb.opskitpro.com` 做成公开仓库，建议采用下面的边界：

- **公开区**：`02_Articles`、`03_Tools`、公开展示用的 `05_Assets`
- **私有区**：原始 Inbox、草稿、内部排障记录、未整理内容
- **推荐方式**：公开仓库只放已整理文章，原始笔记放到私有 vault 或私有仓库
- **约定字段**：公开文章统一带 `published: true` / `draft: false`，并补 `series`、`canonical`

这样可以保证：

- 主站和 KB 只展示可引用内容
- 草稿不会误发
- 公开内容更像文章，而不是草稿堆
- 后续迁移到两个仓库时成本更低

## 🧱 核心模板代码

所有模板已预置在 `04_Templates` 中。在 Obsidian 中安装 `Templates` 库并指向该文件夹即可。

---

> 💡 **小贴士**：请专注于“产出内容”而非“整理知识”。文章发得越多，SEO 流量越大。
