# OpsKitPro KB 开源方案

这是一版适合将 `kb.opskitpro.com` 逐步开源的结构设计。目标不是把所有原始笔记都公开，而是把已经整理好的文章、工具说明和可引用内容开放出去，同时保留草稿和内部记录的编辑自由。

## 推荐原则

1. **公开已发布内容，保留草稿私有**
   - 对外只公开已经整理完成、可以直接引用的文章。
   - Inbox、排障速记、临时草稿保持私有。

2. **公开站点与私有笔记分离**
   - 公共仓库负责 `kb.opskitpro.com` 的正式内容。
   - 私有仓库或本地 vault 负责原始笔记和未完成内容。

3. **文章和文档分层**
   - 文章讲“问题、设计、实现、结论”。
   - 工具说明讲“用途、入口、操作”。
   - 草稿只做内部过渡，不直接发布。

## 推荐仓库模型

### 方案 A: 两仓库

**public repo**
- Quartz 站点
- 已发布文章
- 工具说明
- 公开资源

**private repo**
- Obsidian 原始 vault
- Inbox
- 个人草稿
- 内部排障记录

这是最稳妥的做法。公开仓库只保留可引用内容，私有仓库保留完整思考过程。

### 方案 B: 单仓库 + 严格过滤

如果暂时不想拆仓库，也可以继续用一个仓库，但必须满足：

- 所有未发布内容标记为 `draft: true`
- 私有目录放在 Quartz 忽略范围内
- 只有经过整理的文章才进入公开目录

这个方案迁移成本低，但长期维护时不如两仓库清晰。

## 推荐目录结构

### 公开仓库

```text
content/
  02_Articles/        # 已发布文章
  03_Tools/           # 工具说明、手册
  04_Showcase/        # 截图、案例、公开展示
  90_Archive/         # 已过时但仍可参考的旧文章
docs/
  open-source-strategy.md
  publishing-guidelines.md
  series-index.md
```

### 私有仓库

```text
inbox/
notes/
drafts/
references/
attachments/
```

## frontmatter 约定

建议公开文章统一使用这些字段：

```yaml
---
title: "文章标题"
tags:
  - OpsKitPro
  - SRE
published: true
draft: false
series: "website-check"
source: "kb.opskitpro.com"
canonical: "https://kb.opskitpro.com/..."
---
```

推荐约定：
- `published: true` 表示已发布
- `draft: true` 表示草稿，不应公开
- `series` 用于系列聚合
- `canonical` 用于跨站引用

## 文章分层建议

### 1. 项目总览
适合放：
- 项目背景
- 为什么要做这个站
- 设计原则
- 工程收口

### 2. 模块文章
适合放：
- website-check
- IP / DNS
- services standardization
- Cloudflare Workers 配置

### 3. 工具文章
适合放：
- passgen
- qrgen
- json
- websocket

### 4. 公开工具手册
适合放：
- 入口说明
- 使用方法
- 常见问题
- 引导到主站工具页

## 迁移顺序建议

1. 先保留当前所有已发布文章不动。
2. 将 `01_Notes` 里真正面向公开的内容逐步整理到 `02_Articles`。
3. 将只用于内部追踪的笔记迁移到私有仓库。
4. 给每篇文章补齐 `series` 和 `canonical`。
5. 最后再考虑把仓库完全对外开放。

## 公开边界建议

建议公开：
- 已发布文章
- 工具说明
- 架构图
- 部署经验
- 已清理过的案例复盘

不建议公开：
- 原始 Inbox
- 带环境敏感信息的排障记录
- 未整理的草稿
- 客户相关信息

## 最终目标

最终希望形成这样的结构：

- 主站：`opskitpro.com`
  - 索引
  - 工具入口
  - 文章导读
- KB：`kb.opskitpro.com`
  - 公开文章
  - 工具文档
  - 可引用的技术笔记
- 私有 vault
  - 原始记录
  - 草稿
  - 内部追踪

这样既能公开可复用的内容，又不会把原始工作笔记全部暴露出去。

