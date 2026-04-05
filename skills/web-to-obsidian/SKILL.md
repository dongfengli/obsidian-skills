---
name: web-to-obsidian
description: 将网页内容提取并整理成 Obsidian 笔记。当用户提到"整理成笔记"、"导入笔记"、"网页转笔记"或提供 URL 并要求保存到 Obsidian 时使用此技能。自动提取网页内容并优化为符合 Obsidian 规范的笔记格式。
---

# Web to Obsidian

将网页内容提取并优化整理成 Obsidian 笔记的完整工作流。

## 触发场景

当用户表达以下意图时自动触发：
- "把这个网页整理成笔记"
- "把 https://xxx.com 保存到 Obsidian"
- "导入笔记"
- "网页转笔记"
- 提供 URL + 要求创建笔记

## 工作流

### Step 1: 提取网页内容

使用 defuddle 提取干净的 Markdown 内容：

```bash
defuddle parse <url> --md
```

**重要：必须同时提取图片链接**

```bash
defuddle parse <url> --md | grep -E '!\[.*\]\(https?://[^)]+\)'
```

获取元数据：
```bash
defuddle parse <url> -p title      # 标题
defuddle parse <url> -p domain      # 域名
defuddle parse <url> -p description # 描述
```

### Step 2: 分析内容类型

根据内容特征选择合适的模板：

| 类型特征 | 推荐模板 |
|----------|----------|
| 教程、文档、指南 | 教程模板 |
| 工具、软件、API | 工具模板 |
| 文章、新闻、观点 | 收藏模板 |
| 一般内容 | 通用模板 |

可以用 `obsidian search` 搜索现有笔记，关联相关主题。

### Step 3: 优化笔记内容

使用 Obsidian Markdown 最佳实践：

1. **添加 frontmatter 属性** - title、date、tags
2. **使用 Callouts** - `> [!类型]` 突出重点
3. **添加 Wikilinks** - `[[相关笔记]]` 建立连接
4. **整理结构** - 添加目录、摘要、相关链接
5. **保留图片** - 手动添加图片链接到对应位置（从 Step 1 提取）

### Step 4: 创建笔记

**首选方法**：使用 obsidian-cli 创建笔记

```bash
# 创建笔记（会打开编辑器）
obsidian create name="笔记名称" silent

# 或者指定内容创建
obsidian create name="笔记名称" content="笔记内容"
```

**备选方法**：直接用 write 写入 WebNote 目录（当笔记内容含特殊字符时使用）

```bash
# vault 路径
VAULT_PATH="/Users/lidongfeng/Library/Mobile Documents/iCloud~md~obsidian/Documents/dongfeng"

# 写入笔记到 WebNote 目录
write content="笔记内容" filePath="${VAULT_PATH}/WebNote/笔记名.md"
```

## 模板格式

### 通用模板

```yaml
---
title: {标题}
date: {日期}
tags: [web-import, {域名}]
type: note
---

> [!abstract] 摘要
> {描述}

---

{内容}

---

## 相关链接

- [[Defuddle]]
- [[Obsidian CLI]]
- [[Web Import]]
```

### 教程模板

```yaml
---
title: {标题}
date: {日期}
tags: [web-import, tutorial, {域名}]
type: tutorial
---

> [!abstract] 教程简介
> {描述}

---

%%toc%%

---

## 正文

{内容}

---

## 重点摘要

> [!note] 关键点
> - 要点1
> - 要点2

## 相关链接

- 原文: [{域名}]({URL})
- [[Defuddle]]
- [[Obsidian CLI]]
```

### 工具模板

```yaml
---
title: {标题}
date: {日期}
tags: [web-import, tool, {域名}]
type: tool
---

> [!abstract] 工具简介
> {描述}

---

## 核心功能

{内容}

---

## 使用方法

> [!info] 前置要求
> - 环境要求

```bash
# 安装命令
xxx install
```

```bash
# 基本用法
xxx command
```

## 参数说明

| 参数 | 说明 |
|------|------|
| -a | 参数A说明 |

## 相关链接

- 官网: [{域名}]({URL})
- [[Defuddle]]
- [[Web Import]]
```

### 收藏模板

```yaml
---
title: {标题}
date: {日期}
tags: [web-import, favorite, {域名}]
type: favorite
rating: 0
---

> [!important] 收藏原因
> {描述}

---

## 核心内容

{内容}

---

## 亮点记录

> [!tip] 值得收藏的点
> - 

## 相关链接

- 原文: [{域名}]({URL})
- [[Defuddle]]
- [[Obsidian CLI]]
- [[Web Import]]
```

## 快捷命令

也可以直接使用脚本：

```bash
~/bin/web2obsidian.sh <url> [笔记名] -t [模板类型]
```

模板类型: 通用、教程、工具、收藏

## 依赖工具

- **defuddle** - `npm install -g defuddle` - 网页内容提取
- **obsidian** - Obsidian CLI - 笔记创建与管理
- **obsidian-markdown** - Obsidian 语法参考

## 相关技能

- [[defuddle]] - 网页内容提取
- [[obsidian-cli]] - Obsidian 命令行操作
- [[obsidian-markdown]] - Obsidian 语法
