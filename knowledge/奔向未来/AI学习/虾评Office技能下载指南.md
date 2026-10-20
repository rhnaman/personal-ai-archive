---
title: 虾评 Office 文档技能安装脚本
date: 2026-07-30
tags: [AI, 技能, 虾评, Office]
---

# 🦐 虾评 Office 技能批量下载

在终端中执行此脚本，一次性下载 Word/Excel/PPT/PDF 全套 Office 技能。

## 前置条件
- 终端需支持 `curl` 和 `unzip`
- 你的虾评 API Key（已从 Obsidian 中读取）

## 批量下载脚本

```bash
#!/usr/bin/env bash
set -e

API_KEY="agent-world-7558f276b8757d1b3c47827eeab4b58a31fbc686cf6ec19e"
DOMAIN="https://xiaping.coze.com"
DL_DIR="H:/note1/myhello-word/skills_downloads/office-tools"
mkdir -p "$DL_DIR"

download_skill() {
    local skill_id="$1"
    local name="$2"

    echo "=== 下载: $name ==="

    # Step 1: 获取下载链接
    local resp
    resp=$(curl -sL "$DOMAIN/api/skills/$skill_id/download" \
        -H "Authorization: Bearer $API_KEY")

    local download_url
    download_url=$(echo "$resp" | python3 -c "import sys,json; print(json.load(sys.stdin)['data']['download_url'])")

    if [ -z "$download_url" ] || [ "$download_url" = "null" ]; then
        echo "  ❌ 获取下载链接失败: $resp"
        return 1
    fi

    # Step 2: 下载 ZIP
    local zip_file="$DL_DIR/${name}.zip"
    curl -sL "$download_url" -o "$zip_file"
    echo "  ✅ 已下载: $zip_file"

    # Step 3: 解压
    local unzip_dir="$DL_DIR/${name}"
    mkdir -p "$unzip_dir"
    unzip -o "$zip_file" -d "$unzip_dir"
    echo "  ✅ 已解压: $unzip_dir"

    # Step 4: 预览 SKILL.md
    if [ -f "$unzip_dir/SKILL.md" ]; then
        echo "  📄 SKILL.md 前5行:"
        head -5 "$unzip_dir/SKILL.md"
        echo "  ..."
    fi

    echo ""
}

echo "🦐 开始批量下载 Office 技能..."
echo ""

# === 试用版（免费） ===
echo "--- 试用版技能（免费） ---"

# Word全能工具箱 v3.1.1
download_skill "0122adc9-f650-4806-89ae-8dbd730c3fa9" "word-toolbox"

# Excel全能工具箱 v5.0.1
download_skill "892200ca-e22a-4a4e-aaf6-9fe85cbd72ec" "excel-toolbox"

# 通用文档格式转换器 v2.1.1
download_skill "f6281302-52f8-4aca-a494-3a475c14dc78" "doc-converter"

# ppt-master (文档→PPTX)
download_skill "4c939616-f4de-4eff-ba91-dccee94a0864" "ppt-master"

# === 正式版（消耗虾米） ===
echo "--- 正式版技能（需虾米） ---"
echo "以下技能为正式版，每次下载消耗 2 虾米。当前余额: 26 🦐"
echo "按 Ctrl+C 可取消。按 Enter 继续下载..."

# MiniMax PPT生成 (2虾米)
download_skill "c7730b56-63fb-4c05-aed9-5e8afd3e9c19" "minimax-ppt"

# MiniMax PDF生成 (2虾米)
download_skill "0589b856-cd4e-4095-84c9-0617f81684d8" "minimax-pdf"

# 可选：公文格式化大师 (2虾米)
download_skill "2abdc010-5f2e-4164-9ca9-80b3f7fdad36" "gongwen-formatter"

echo ""
echo "🎉 全部下载完成！文件位于: $DL_DIR"
```

## 技能清单

| 技能 | 版本 | 命令数 | 类型 | 消耗 |
|------|------|--------|------|------|
| Word全能工具箱 | v3.1.1 | 40+ | 试用版 | 免费 |
| Excel全能工具箱 | v5.0.1 | 75+ | 试用版 | 免费 |
| 通用文档格式转换器 | v2.1.1 | 30+ 路径 | 试用版 | 免费 |
| ppt-master | v1.0.0 | 文档→PPTX | 试用版 | 免费 |
| MiniMax PPT生成 | v1.0.3 | 创建+编辑 | 正式版 | 2 🦐 |
| MiniMax PDF生成 | v1.0.3 | 双引擎 | 正式版 | 2 🦐 |
| 公文格式化大师 | v1.0.6 | GB/T 9704 | 正式版 | 2 🦐 |

**总消耗**：免费×4 + 正式版×3 = 最多 6 虾米（余额 26，足够）

## 下载后步骤

1. 阅读每个技能的 SKILL.md
2. 安装依赖（如 `python-docx`、`openpyxl`、`PptxGenJS` 等）
3. 测试核心功能
4. 发表评测赚回虾米
