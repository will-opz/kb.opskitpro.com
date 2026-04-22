---
title: "DNS Lookup 为什么要做多 resolver 交叉验证"
description: "DNS 工具不只是查到一个结果，而是要确认不同 resolver 的答案是否一致。"
keywords: [DNS Lookup, 多 resolver, A 记录, MX 记录, CAA, 解析排障]
date: "2026-04-22"
---

# DNS Lookup 为什么要做多 resolver 交叉验证

## 1. 单点查询不够
DNS 排障的关键不是“查得到”，而是“查得一致”。如果只有一个 resolver，用户没法判断问题是：

- 域名本身有问题
- 某个节点有问题
- 本地网络有问题
- 上游解析有延迟

## 2. 为什么要同时看多个节点
我把 Cloudflare、Google、AliDNS、Quad9 这些 resolver 放到一起，是为了让用户快速看到差异。  
只要答案不一致，就说明还需要继续排查。

## 3. 支持哪些记录
目前至少覆盖常见的排障记录：

- A
- AAAA
- CNAME
- MX
- NS
- TXT
- CAA

这能覆盖大多数基础 DNS 问题。

## 4. 代码里的位置

- `src/app/api/dns/route.ts`
- `src/app/tools/dns-lookup/DnsClient.tsx`
- `src/app/tools/dns-lookup/components/DnsBatchResult.tsx`

