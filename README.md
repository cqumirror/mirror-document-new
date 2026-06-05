# mirror-document-new

重庆大学开源软件镜像站（CQU Mirror）前端内容仓库，以 Git 子模块方式引入 [mirror-frontend-new](https://github.com/cqumirror/mirror-frontend-new) 的 `content/` 目录。

本仓库包含两类内容：**帮助文档**（镜像使用帮助）和**新闻文章**（公告、更新通知等）。所有内容以 MDX 格式编写，前端通过 `import.meta.glob` 在构建时自动发现并加载。

---

## 目录结构

```
mirror-document-new/
├── docs/
│   └── mdx/
│       ├── zh/          ← 中文帮助文档
│       │   ├── ubuntu.mdx
│       │   ├── archlinux.mdx
│       │   └── ...
│       └── en/          ← 英文帮助文档
│           ├── ubuntu.mdx
│           ├── archlinux.mdx
│           └── ...
└── news/
    └── mdx/
        ├── zh/          ← 中文新闻
        │   ├── 2026-03-17-new-mirrors.mdx
        │   └── ...
        └── en/          ← 英文新闻
            ├── 2026-03-17-new-mirrors.mdx
            └── ...
```

**规则**：`zh/` 和 `en/` 下的文件名必须一一对应。前端会根据当前语言加载对应目录下的同名文件。

---

## 帮助文档（docs/mdx/）

帮助文档为某个镜像的使用指南，文件名即镜像 ID（如 `ubuntu.mdx` 对应 `/mirrors/ubuntu`）。

### 文件命名

- 文件名与 `public/data/local_data.json` 中的镜像 ID 一致
- 扩展名 `.mdx`
- 示例：`ubuntu.mdx`、`archlinux.mdx`、`crates.io-index.mdx`

### 内容结构

帮助文档使用标准 Markdown，推荐按以下结构组织：

```markdown
## 镜像名称 使用帮助

## 地址

https://mirrors.cqu.edu.cn/镜像路径/

## 说明

简要说明该镜像是什么。

## 收录架构

x86_64, aarch64

## 收录版本

列出收录的版本信息。

## 使用说明

具体的配置步骤，包含命令示例：

​```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i 's|http://archive.ubuntu.com|https://mirrors.cqu.edu.cn|g' /etc/apt/sources.list
​```

### 子标题

使用说明可以有多个子章节（h3）。

## 相关链接

- [官方网站](https://example.org)
- [官方文档](https://example.org/docs)
```

### 各节说明

| 章节 | 说明 | 是否必须 |
|------|------|----------|
| `## 地址` | 镜像站中该镜像的 URL | 推荐 |
| `## 说明` | 一句话描述镜像内容 | 推荐 |
| `## 收录架构` | 支持的 CPU 架构（如 x86_64、aarch64） | 可选 |
| `## 收录版本` | 收录的版本范围或说明 | 可选 |
| `## 使用说明` | 核心内容：配置步骤和命令 | 必须 |
| `## 相关链接` | 官方网站、文档等外部链接 | 可选 |

### Markdown 语法支持

- 标准 Markdown 语法（标题、列表、链接、加粗、斜体等）
- 围栏代码块（` ```bash `、` ``` ` 等），前端会做语法高亮
- 表格（GFM 语法）
- 行内代码（`` ` ``）
- 锚点链接（`[链接文字](#章节标题)`）

### 注意事项

- URL 使用 `https://mirrors.cqu.edu.cn/` 开头
- 命令示例放在围栏代码块中，标注语言类型（通常为 `bash`）
- 不要使用 HTML 标签（如 `<kbd>`、`<details>`），前端不保证渲染
- 不要使用 `{{% notice %}}` 等 Hugo 短代码，已迁移为纯 Markdown
- 不需要 YAML frontmatter，直接从内容开始

---

## 新闻文章（news/mdx/）

新闻文章用于发布镜像站公告、新镜像上线通知、维护公告等。

### 文件命名

格式：`YYYY-MM-DD-slug.mdx`

- 日期部分与 `meta.date` 一致
- slug 为英文短横线分隔的简短标识
- 示例：`2026-03-17-new-mirrors.mdx`、`2023-05-16-new-frontend.mdx`

### meta 导出

每个新闻文件**必须**在开头导出一个 `meta` 对象：

```javascript
export const meta = {
    title: "中文标题",
    titleEn: "English Title",
    date: "2026-03-17",
    author: "作者名",
    summary: "中文摘要，简要描述文章内容",
    summaryEn: "English summary, briefly describing the article",
    tags: ["标签1", "标签2"],
}
```

### meta 字段说明

| 字段 | 类型 | 是否必须 | 说明 |
|------|------|----------|------|
| `title` | string | **必须** | 中文标题 |
| `titleEn` | string | **必须** | 英文标题 |
| `date` | string | **必须** | 发布日期，格式 `YYYY-MM-DD` |
| `author` | string | 推荐 | 作者姓名 |
| `summary` | string | **必须** | 中文摘要（一行） |
| `summaryEn` | string | **必须** | 英文摘要（一行） |
| `tags` | string[] | 推荐 | 标签数组，如 `["新镜像", "公告"]` |

### 正文

meta 导出之后是标准 Markdown 正文：

```markdown
export const meta = {
    title: "新镜像公告",
    titleEn: "New Mirror Announcement",
    date: "2026-03-17",
    author: "Haoran Tan",
    summary: "新增GitHub Release镜像",
    summaryEn: "New GitHub Release mirror added",
    tags: ["新镜像", "公告"],
}

为更好地满足需求，镜像站添加了 [github-release](/github-release) 目录。

目前已同步：

- [Office Tool Plus](/mirrors/github-release/office-tool)
- [OBS Studio](/mirrors/github-release/obs-studio)

更多软件即将上线，敬请期待！
```

### 注意事项

- 正文中的链接可以使用相对路径，如 `/mirrors/ubuntu`、`/github-release`
- 中文和英文版本的正文内容应语义一致，但不要求逐字翻译
- meta 中的 `tags` 使用中文标签（前端会直接展示）
- 不需要 YAML frontmatter（使用 `export const meta` 代替）

---

## 添加新镜像帮助文档

1. 在 `docs/mdx/zh/` 下创建 `镜像ID.mdx`
2. 在 `docs/mdx/en/` 下创建同名文件
3. 按上述结构编写内容
4. 确保主项目 `public/data/local_data.json` 中对应镜像的 `helpUrl` 字段已设置

```bash
# 示例：添加 crates.io 镜像帮助
echo '## crates.io 镜像使用帮助' > docs/mdx/zh/crates.io-index.mdx
echo '## crates.io Mirror Usage Guide' > docs/mdx/en/crates.io-index.mdx
```

## 添加新闻文章

1. 在 `news/mdx/zh/` 下创建 `YYYY-MM-DD-slug.mdx`
2. 在 `news/mdx/en/` 下创建同名文件
3. 两个文件都必须有 `export const meta` 导出
4. 编写正文内容

```bash
# 示例：添加维护公告
touch news/mdx/zh/2026-06-10-maintenance.mdx
touch news/mdx/en/2026-06-10-maintenance.mdx
```

## 提交与更新

内容修改后提交到本仓库，主项目通过更新子模块指针来获取最新内容：

```bash
# 在主项目中更新子模块
cd mirror-frontend-new
git submodule update --remote content
git add content
git commit -m "chore: update content submodule"
git push
```

---

## 与前端的集成

前端通过以下方式加载本仓库内容：

- **帮助文档**：`src/docs/index.ts` 使用 `import.meta.glob('../../content/docs/mdx/{zh,en}/*.mdx')` 构建时发现所有文件，按需懒加载
- **新闻文章**：`src/news/index.ts` 使用 `import.meta.glob('../../content/news/mdx/{zh,en}/*.mdx')` 构建时发现所有文件，列表页立即加载

文件名（不含扩展名）作为路由标识：`ubuntu.mdx` → `/mirrors/ubuntu`，`2026-03-17-new-mirrors.mdx` → `/news/2026-03-17-new-mirrors`。

---

## 相关文档 / Related Docs

- [项目主页](../README.md) — 功能特性、技术栈、开发调试
- [运行时数据文件说明](../public/data/README.md) — announcements、local_data、popular-mirrors 等 JSON 文件格式
- [GitHub Release 子项目配置](../public/data/github-release/README.md) — 添加新 GitHub Release 镜像
