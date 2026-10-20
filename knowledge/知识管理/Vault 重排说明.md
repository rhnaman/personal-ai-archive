---
title: Vault 重排说明
tags: [系统, 索引]
created: 2026-07-04
updated: 2026-07-04
---

# Vault 目录重排说明

## 执行时间
2026-07-04

## 重排原则
按三大块组织：
1. **工作** — 本店企划营销全部业务资料
2. **奔向未来 / AI学习** — Hermes、虾评、Agent 等 AI 学习内容
3. **个人** — 职业发展、公司文化、日程备忘、读书笔记

## 主要变更
- 删除/合并空目录与重复目录（如「老板的讲话和公司的指导」合并为「公司指导」）
- 移动根目录散落文件到对应分类
- 「创意工厂」中的 Agent 团队、AI 笔记迁移到「奔向未来/AI学习」
- 「日程管理」「手机记事本」迁移到「个人」
- 「日报管理」迁移到「工作/团队管理/日报」
- 「memory」系统笔记迁移到「系统/daily-notes」
- 修复大部分内部 Wiki 链接
- 更新 [[知识管理/主页]] 与 [[知识管理/目录]]

## 日志文件
所有操作日志保存在 `.temp/` 目录：
- `vault_snapshot_before_reorg.txt` — 重排前快照
- `reorg_move_log.txt` — 文件移动日志
- `reorg_link_fix_log.txt` — 链接修复日志
- `reorg_broken_links_final.txt` — 最终破损链接清单
- `reorg_comparison_report.txt` — 前后对比报告

## 注意事项
- Git 仓库索引原本已损坏，本次重排未依赖 git；建议手动初始化新的版本控制。
- 仍有少量历史链接指向已删除文件，详见 `reorg_broken_links_final.txt`。
