# Python脚本工具配置

本指南介绍如何为Obsidian配置Python脚本工具，以便在笔记中运行Python脚本。

## 1. Python环境配置

### 验证Python安装
确保Python已正确安装在系统中：
- Python版本：3.12.10
- 安装路径：`C:\Users\rhnam\AppData\Local\Programs\Python\Python312\`

### 设置环境变量（可选）
为使Python在命令行中全局可用，可将以下路径添加到系统PATH：
1. `C:\Users\rhnam\AppData\Local\Programs\Python\Python312\`
2. `C:\Users\rhnam\AppData\Local\Programs\Python\Python312\Scripts\`

## 2. 脚本目录结构

本库已配置以下目录：

```
scripts/          # Python脚本存放目录
  ├── hello.py    # 示例脚本
  └── utils.py    # 工具函数
Templates/        # 模板目录
  ├── python-script-template.md
  └── python-output-template.md
```

## 3. 运行Python脚本的方法

### 方法1：使用命令面板执行脚本
1. 打开命令面板 (`Ctrl+P`)
2. 输入"终端"或"命令"
3. 运行：`python scripts/hello.py`

### 方法2：在笔记中嵌入代码块
使用Obsidian的代码块语法：

````markdown
```python
print("Hello from Python in Obsidian!")
```
````

### 方法3：使用Templater插件（如安装）
如果安装了Templater插件，可创建模板自动运行Python脚本。

## 4. 示例脚本

### 基础示例 (`scripts/hello.py`)
```python
#!/usr/bin/env python3
"""
基础示例脚本
在Obsidian中运行Python的简单示例
"""

def main():
    print("=" * 50)
    print("Python脚本在Obsidian中运行成功!")
    print("=" * 50)
    print(f"Python版本: {__import__('sys').version}")
    print(f"当前时间: {__import__('datetime').datetime.now()}")
    print("=" * 50)

    # 简单计算示例
    numbers = [1, 2, 3, 4, 5]
    total = sum(numbers)
    average = total / len(numbers)

    print(f"数字列表: {numbers}")
    print(f"总和: {total}")
    print(f"平均值: {average:.2f}")
    print("=" * 50)

    # 文件操作示例（安全模式）
    import os
    vault_path = os.path.dirname(os.path.abspath(__file__))
    print(f"库路径: {vault_path}")

    # 统计库中的文件
    md_files = [f for f in os.listdir(vault_path) if f.endswith('.md')]
    print(f"库中的Markdown文件数量: {len(md_files)}")
    print("=" * 50)

if __name__ == "__main__":
    main()
```

### 数据处理示例 (`scripts/data_processor.py`)
```python
#!/usr/bin/env python3
"""
数据处理示例
演示如何在Obsidian中处理数据
"""

import json
import csv
from pathlib import Path

def process_json_data():
    """处理JSON数据示例"""
    data = {
        "projects": [
            {"name": "项目A", "status": "进行中", "progress": 75},
            {"name": "项目B", "status": "已完成", "progress": 100},
            {"name": "项目C", "status": "待开始", "progress": 0}
        ],
        "last_updated": "2024-01-15"
    }

    print("JSON数据处理示例:")
    print(json.dumps(data, indent=2, ensure_ascii=False))
    return data

def calculate_statistics(data):
    """计算统计数据"""
    projects = data["projects"]
    total = len(projects)
    completed = sum(1 for p in projects if p["status"] == "已完成")
    in_progress = sum(1 for p in projects if p["status"] == "进行中")
    pending = sum(1 for p in projects if p["status"] == "待开始")

    stats = {
        "total_projects": total,
        "completed": completed,
        "in_progress": in_progress,
        "pending": pending,
        "completion_rate": (completed / total * 100) if total > 0 else 0
    }

    return stats

def main():
    print("=" * 50)
    print("数据处理脚本")
    print("=" * 50)

    # 处理JSON数据
    data = process_json_data()

    # 计算统计信息
    stats = calculate_statistics(data)

    print("\n项目统计:")
    for key, value in stats.items():
        if isinstance(value, float):
            print(f"  {key}: {value:.1f}%")
        else:
            print(f"  {key}: {value}")

    print("=" * 50)

if __name__ == "__main__":
    main()
```

## 5. 模板使用

### Python脚本模板 (`Templates/python-script-template.md`)
在创建新的Python相关笔记时使用此模板：

```markdown
---
created: {{date}} {{time}}
tags: [python, script]
---

# {{title}}

## 脚本目的


## 代码实现

```python
# 在这里编写Python代码
```

## 使用说明

1.
2.
3.

## 示例输出

```

## 6. 高级用法

### 与Dataview插件集成
可以使用Python处理Dataview查询的数据：

```python
# 读取Markdown文件并解析frontmatter
import frontmatter
import glob

def analyze_notes():
    """分析笔记库中的文件"""
    notes = []
    for file in glob.glob("*.md"):
        with open(file, 'r', encoding='utf-8') as f:
            post = frontmatter.load(f)
            notes.append({
                'file': file,
                'metadata': post.metadata,
                'content_length': len(post.content)
            })
    return notes
```

### 自动化任务
创建自动化脚本处理日常任务：
- 批量重命名文件
- 生成报告摘要
- 数据转换和清洗

## 7. 故障排除

### 常见问题
1. **Python命令未找到**
   - 使用完整路径：`C:\Users\rhnam\AppData\Local\Programs\Python\Python312\python.exe`
   - 或将Python添加到系统PATH

2. **权限错误**
   - 确保脚本文件有执行权限
   - 在管理员模式下运行终端

3. **导入错误**
   - 使用虚拟环境管理依赖
   - 安装所需包：`pip install package-name`

### 获取帮助
- 查看Python官方文档
- 在Obsidian社区论坛搜索相关问题
- 查看本库中的示例脚本

---

**最后更新**: {{date}} {{time}}
**维护者**: 系统自动配置