---
title: "website-check 为什么改成并行探测"
description: "DNS、HTTP、SSL、CDN 本质上不是一条长流程，而是多个信号一起看，最后再汇总成结论。"
keywords: [website-check, 并行探测, 诊断流程, SSL, CDN, HTTP]
date: "2026-04-22"
---

# website-check 为什么改成并行探测

## 1. 串行流程太像“流水线”
传统诊断页很容易做成：

1. DNS
2. HTTP
3. SSL
4. CDN
5. 汇总结果

这种设计的问题是，用户会一直等“下一步”，却还没看到真正有用的判断。

## 2. 真实的排障更像并行工作
实际排障时，DNS、HTTP、SSL、CDN 往往是同时看：

- DNS 有没有解析成功
- HTTP 有没有响应
- SSL 证书是不是异常
- CDN 是否接管请求

所以我把页面改成了“摘要先行、明细折叠”的形式，让用户先拿到结论，再决定要不要看细节。

## 3. 页面上是怎么表现的
现在的结构更像：

- 顶部输入
- 中间摘要
- 下方要点和明细

而不是一条长长的五步流水线。

## 4. 代码里的位置

- `src/app/tools/website-check/WebsiteCheckClient.tsx`
- `src/app/tools/website-check/page.tsx`
- `src/app/api/diagnostic/route.ts`

