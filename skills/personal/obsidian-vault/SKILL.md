---
name: obsidian-vault
description: 在 Obsidian 知识库中搜索、创建和管理笔记，支持 wikilink 和索引笔记。当用户想要在 Obsidian 中查找、创建或组织笔记时使用。
---

# Obsidian 知识库

## 知识库位置

`/mnt/d/Obsidian Vault/AI Research/`

根级基本扁平。

## 命名约定

- **索引笔记**：聚合相关主题（例如 `Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
- 所有笔记名使用 **Title Case**
- 不用文件夹组织——改用链接和索引笔记

## 链接

- 使用 Obsidian `[[wikilinks]]` 语法：`[[Note Title]]`
- 笔记在底部链接到依赖/相关笔记
- 索引笔记就是 `[[wikilinks]]` 列表

## 工作流

### 搜索笔记

```bash
# 按文件名搜索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 按内容搜索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或直接对知识库路径使用 Grep/Glob 工具。

### 创建新笔记

1. 文件名使用 **Title Case**
2. 将内容写为一个学习单元（按知识库规则）
3. 在底部添加 `[[wikilinks]]` 链接到相关笔记
4. 如果属于编号序列，使用层级编号方案

### 查找相关笔记

在整个知识库中搜索 `[[Note Title]]` 来查找反向链接：

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找索引笔记

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
