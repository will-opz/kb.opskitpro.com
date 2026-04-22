---
title: "IP Lookup 为什么要返回结构化兜底结果"
description: "IP 页不应该因为上游定位库短暂不可用就直接失败，而是要保留能确认的部分信息。"
keywords: [IP Lookup, 结构化回退, Cloudflare Context, GeoIP, SRE]
date: "2026-04-22"
---

# IP Lookup 为什么要返回结构化兜底结果

## 1. 不能让页面只剩一个错误
IP 页的价值不是“查一次能拿到多少字段”，而是让用户快速知道：

- 我现在从哪里访问
- 是不是代理 / VPN
- 位置和 ASN 能不能先确认

如果上游 GeoIP 服务短暂失效，页面不应该直接白屏或者只剩错误码。

## 2. 什么叫结构化兜底
结构化兜底的意思是：

- 能确认的先展示
- 不确定的明确标记
- 数据来源也要说明

这样用户至少还能继续排障，而不是被迫重试。

## 3. 为什么它很重要
在边缘环境里，外部服务抖动很常见。结构化回退的价值就在于：

- 保持页面可读
- 保持信息可信
- 保持结论可复制

## 4. 代码里怎么做

- `src/app/api/ip/route.ts`
- `src/app/tools/ip-lookup/IPLookupClient.tsx`
- `src/app/api/ip/__tests__/route.test.ts`

