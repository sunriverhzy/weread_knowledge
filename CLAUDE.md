# 微信读书 Wiki — Schema & Operating Instructions

> This file is the **single source of truth** for how the LLM should build,
> maintain, and query this knowledge base. It is the "brain" of the wiki.
>
> Philosophy (after Andrej Karpathy's LLM Wiki pattern):
> **Compile knowledge at ingest time. Query the compiled wiki — not the raw sources.**

---

## 🎯 Purpose

This wiki is a personal reading knowledge base synced with WeRead (微信读书),
covering all books read, reading notes, highlights, and reading insights.

The knowledge domains are organized by book categories:

- **文学** — 外国文学、中国文学、诗歌、小说
- **历史** — 世界史、中国史、历史小说
- **哲学宗教** — 哲学读物、宗教
- **科学技术** — 自然科学、科学科普
- **经济理财** — 财经、经济学
- **心理** — 心理学研究、积极心理学
- **社会文化** — 社科、法律、政治
- **人物传记** — 传记、回忆录
- **医学健康** — 健康、养生
- **其他** — 未分类

The goal is to **compile reading knowledge at ingest time** and **query the
compiled wiki** — not to re-read raw highlights every time.

---

## 🏛️ Three-Layer Architecture

```
┌──────────────────────────────────────────┐
│  Layer 3: Schema  (this file: CLAUDE.md) │  ← Rules, templates, workflows
├──────────────────────────────────────────┤
│  Layer 2: Wiki    (wiki/*)               │  ← LLM-generated, interlinked
├──────────────────────────────────────────┤
│  Layer 1: Sources (sources/*)            │  ← Synced from WeRead API
└──────────────────────────────────────────┘
```

- **Sources** are synced data from WeRead. Do not manually edit.
- **Wiki** is fully regenerable from Sources + Schema.
- **Schema** is hand-curated by the human owner.

---

## 📂 Directory Structure

| Path | Description |
|------|-------------|
| `CLAUDE.md` | This schema file. |
| `README.md` | Human-facing quick-start guide. |
| `sources/` | Layer 1: synced data from WeRead API. |
| `sources/shelf.json` | Full bookshelf snapshot. |
| `sources/readdata.json` | Reading statistics snapshot. |
| `sources/notebooks/` | Per-book notes and highlights (JSON). |
| `wiki/` | Layer 2: LLM-maintained knowledge base. |
| `wiki/index.md` | Content catalog — navigation hub. |
| `wiki/overview.md` | High-level reading profile and knowledge map. |
| `wiki/books/{category}/` | One page per book. |
| `wiki/concepts/` | One page per concept or theme. |
| `wiki/authors/` | One page per notable author. |
| `wiki/topics/` | Cross-book topic synthesis pages. |
| `wiki/reading-lists/` | Curated reading paths by topic. |
| `wiki/stats/` | Reading statistics and trends. |
| `wiki/log.md` | Operations log (append-only). |

**Category folder values** (use lowercase, Chinese pinyin kebab-case):
`wenxue`, `lishi`, `zhexue`, `kexue`, `jingji`, `xinli`, `shehui`, `zhuanji`, `jiankang`, `other`

---

## 🔗 Linking Rules

- Use `[[PageTitle]]` (wikilinks) for all internal cross-references.
- Every **book page** MUST link to at least one concept or topic page.
- Every **concept page** MUST link to at least one book page.
- When referencing a book inline, use the slug: `[[cormac-mccarthy-blood-meridian]]`.
- When a concept or author is mentioned for the first time in a page, link it.
- Prefer linking to specific section headings when useful: `[[PageTitle#Section]]`.

---

## 🏷️ Tagging Rules

All pages MUST include YAML frontmatter. Tags should be chosen from
the controlled vocabulary below.

**Category tags**
`文学`, `历史`, `哲学`, `宗教`, `科学`, `科普`, `经济`, `财经`, `心理`,
`社会`, `法律`, `传记`, `健康`, `小说`, `诗歌`

**Theme tags**
`人性`, `自由`, `权力`, `文明`, `进化`, `认知`, `情感`, `社会制度`,
`经济学`, `技术`, `自然`, `宗教信仰`, `死亡`, `战争`, `爱情`,
`心理学`, `哲学思考`, `历史反思`, `科学方法`, `生物学`

**Reading status tags**
`reading`, `finished`, `queued`, `key-book`, `revisit`

---

## 📋 Page Templates

### Template A — Book Page

- **File path:** `wiki/books/{category}/{slug}.md`
- **Slug format:** `{author-lastname}-{keyword}`
  - Example: `dawkins-selfish-gene`, `mccarthy-blood-meridian`

```markdown
---
title: "[书名]"
slug: "[slug]"
author: "[作者]"
translator: "[译者]"
publisher: "[出版社]"
category: "[分类]"
tags: [tag1, tag2, ...]
status: [reading | finished | queued]
rating: [WeRead评分/100]
progress: [阅读进度%]
book_id: "[WeRead bookId]"
highlights: [划线数]
thoughts: [想法数]
read_time: "[阅读时长]"
last_read: "[最近阅读日期]"
---

# [书名]

## 基本信息
- **作者:** [作者]
- **译者:** [译者（如有）]
- **出版社:** [出版社]
- **分类:** [分类]
- **字数:** [总字数]
- **评分:** [评分/100]

## 内容概要
> [一段话概括本书核心内容和价值]

## 核心观点
1. [核心观点1]
2. [核心观点2]
3. [核心观点3]

## 精华摘录
> [重要划线/高亮内容]

## 个人想法
[阅读中的个人思考和感悟]

## 关联
- **相关书籍:** [[book]], [[book]]
- **相关概念:** [[concept]], [[concept]]
- **同作者:** [[book]]

## 阅读数据
- **阅读进度:** X%
- **阅读时长:** X小时Y分钟
- **划线数:** N
- **想法数:** N
```

---

### Template B — Concept/Theme Page

- **File path:** `wiki/concepts/{slug}.md`

```markdown
---
title: "[概念名]"
aliases: ["[别名1]", "[别名2]"]
domain: [category tags]
tags: [tag1, tag2]
source_count: [N]
last_updated: [YYYY-MM-DD]
---

# [概念名]

## 定义
[清晰、精确的定义]

## 在阅读中的体现

| 书籍 | 相关观点 |
|------|----------|
| [[book-slug]] | [该书对此概念的阐述] |

## 个人理解
[基于多本书的综合思考]

## 相关概念
- [[concept-1]]
- [[concept-2]]
```

---

### Template C — Author Page

- **File path:** `wiki/authors/{slug}.md`

```markdown
---
title: "[作者名]"
nationality: "[国籍]"
domain: [主要创作领域]
books_read: [N]
---

# [作者名]

## 简介
[作者背景和主要成就]

## 已读作品
- [[book-slug]] — [一句话评价]

## 写作风格与特点
[该作者的写作风格总结]

## 核心思想
[该作者的核心思想观点]
```

---

### Template D — Topic Synthesis Page

- **File path:** `wiki/topics/{slug}.md`

```markdown
---
title: "[主题名]"
books: ["[[book-a]]", "[[book-b]]"]
last_updated: [YYYY-MM-DD]
---

# [主题名]

## 概述
[该主题的整体描述]

## 多书视角

### [[Book A]] 的观点
[...]

### [[Book B]] 的观点
[...]

## 综合分析
[跨书籍的主题综合分析]

## 推荐阅读顺序
1. [[book-slug]] — *[为什么先读这本]*
2. [[book-slug]] — *[这本补充了什么]*
```

---

## ⚙️ Ingest Workflow — Step by Step

When syncing from WeRead (typically via API), follow this procedure:

1. **Sync** bookshelf data via `/shelf/sync` → save to `sources/shelf.json`.
2. **Sync** reading stats via `/readdata/detail` → save to `sources/readdata.json`.
3. **For each book** with notes:
   a. Fetch highlights via `/book/bookmarklist`.
   b. Fetch thoughts via `/review/list/mine`.
   c. Save raw data to `sources/notebooks/{bookId}.json`.
4. **Create/Update** book page at `wiki/books/{category}/{slug}.md` using **Template A**.
5. **Identify** key concepts and themes. For each:
   - If exists → **update** (add this book's perspective).
   - If not → **create** using **Template B**.
6. **Update** author pages for authors with 2+ books in the wiki.
7. **Update** `wiki/index.md`: add the book under correct category.
8. **Update** `wiki/overview.md` if reading profile has significantly changed.
9. **Log** the operation in `wiki/log.md`.

---

## 🔍 Query Behavior

When answering a question from the wiki:

1. **Load `wiki/index.md` first** to understand what's available.
2. **Load relevant wiki pages** (books, concepts, topics) — NOT raw sources.
3. **Answer from the synthesized wiki content.**
4. If a good answer requires cross-referencing 3+ books,
   **offer to save it** as a topic page in `wiki/topics/`.
5. **Always cite specific wiki pages:** "According to [[book-slug]]..."

---

## 🔄 WeRead API Integration

This wiki integrates with WeRead via the Agent API Gateway.

- **Endpoint:** `POST https://i.weread.qq.com/api/agent/gateway`
- **Auth:** `Authorization: Bearer $WEREAD_API_KEY`
- **Key APIs used:**
  - `/shelf/sync` — bookshelf data
  - `/readdata/detail` — reading statistics
  - `/user/notebooks` — notebooks overview
  - `/book/bookmarklist` — highlights per book
  - `/review/list/mine` — thoughts per book
  - `/book/info` — book details
  - `/book/getprogress` — reading progress

---

## 📝 Operating Principles

1. **Never manually edit files in `sources/`** — they are API-synced.
2. **Always include YAML frontmatter** on wiki pages.
3. **Prefer updating existing pages over creating duplicates.**
4. **Every book page should be readable standalone.**
5. **Keep `index.md` in sync** — it's the entry point for every query.
6. **When in doubt, be concise.** A short, correct wiki page beats a long, hand-wavy one.
7. **Cross-link generously** — the value of a wiki is in its connections.

---

## 🧭 Four Core Operations — Quick Reference

| Operation | Trigger | Action |
|-----------|---------|--------|
| **Init**   | First run | Read `CLAUDE.md`, create directory skeleton + empty `index.md` / `overview.md`. |
| **Sync**   | "sync from WeRead" | Pull latest data from WeRead API, update sources and wiki pages. |
| **Query**  | Any question | Follow the Query Behavior rules; cite wiki pages. |
| **Lint**   | `/lint` | Check for broken links, orphan pages, missing concepts. |
