---
title: "OpsKitPro 为什么最终跑在 Cloudflare Workers 上"
description: "主站选择 Cloudflare Workers，是为了让边缘诊断更接近真实场景，同时保持部署链路单一。"
keywords: [Cloudflare Workers, OpenNext, 边缘部署, Next.js, OpsKitPro]
date: "2026-04-22"
---

# OpsKitPro 为什么最终跑在 Cloudflare Workers 上

## 1. 这个项目最适合边缘部署
OpsKitPro 做的是诊断和排障，天然要接近真实访问路径。  
如果诊断工具本身离用户太远，就很难看清网络和边缘行为。

## 2. OpenNext 负责把 Next.js 接到 Cloudflare
项目当前的部署方式是：

- Next.js App Router
- `@opennextjs/cloudflare`
- Cloudflare Workers

这样可以保留 Next.js 的开发体验，同时把最终运行环境放到边缘。

## 3. 为什么不把所有内容都塞进主站
主站只保留必须的页面和工具，长文档、技术笔记、工具说明则交给 KB 静态站点。  
这样做的好处是：

- 主站更轻
- 博客内容更灵活
- 知识库可以独立演进

## 4. 代码和配置

- `wrangler.jsonc`
- `open-next.config.ts`
- `src/app/layout.tsx`
- `README.md`
- `OpsKitPro_Backlog.md`

