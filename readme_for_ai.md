# readme_for_ai

> 面向后续 AI 协作的项目架构文档。先读本文件定位方向，再按需读子目录文档。

## 1. 项目定位

基于 **Quarto** 的个人学术主页，部署到 **GitHub Pages**。

渲染链路：`源文件 (*.qmd / *.md) → _quarto.yml → quarto render → docs/ → GitHub Pages`

核心原则：**改源文件，不手改 `docs/`**。

## 2. 技术栈

| 层 | 技术 |
|---|---|
| 站点框架 | Quarto (website project) |
| 页面源码 | `.qmd` (Quarto Markdown) |
| CV 生成 | `awesomecv-typst` 扩展 → PDF |
| 样式 | SCSS (cosmo 主题 + `styles.scss`) + `custom.css` 覆盖层 |
| 部署 | GitHub Pages，服务 `docs/` 目录 |

## 3. 顶层目录结构

```
├── _quarto.yml          # 站点总配置（navbar, footer, theme, output）
├── index.qmd            # 首页（hero profile + 简介 + 论文摘要 + 项目）
├── publications.qmd     # 论文完整列表页
├── interests.qmd        # 研究方向页
├── cv.qmd               # CV 源文件 → 输出 docs/cv.pdf
├── about.qmd            # 占位页（未在 navbar 暴露）
├── styles.scss          # 全站主题基线
├── custom.css           # 局部样式覆盖/补丁
├── image.png            # 首页主头像
├── travel.qmd           # 旅行轨迹页（按时间倒序的旅行卡片）
├── images/              # favicon (site-logo.png)、备用头像
│   └── travel/          # 旅行照片（命名：地点-序号.jpg）
├── paper.pdf            # 静态附件
├── rubrichub.pdf        # 论文 PDF 附件
├── _extensions/         # Quarto 扩展（awesomecv）
├── webr/                # webR 扩展（当前未使用，可忽略）
├── docs/                # 部署产物（quarto render 生成，不手改）
├── test.md              # 本地草稿页（默认不参与公开渲染）
├── email_template.txt   # 对外联络模板
├── tmp_gallery.html     # 草稿/辅助文件
├── notes.txt            # 草稿笔记
├── AGENTS.md            # 仓库 AI 编码规范
└── README.md            # 面向人类的 README
```

## 4. 源文件到输出映射

| 源文件 | 输出 |
|---|---|
| `index.qmd` | `docs/index.html` |
| `publications.qmd` | `docs/publications.html` |
| `interests.qmd` | `docs/interests.html` |
| `cv.qmd` | `docs/cv.pdf`（注意：PDF 不是 HTML） |
| `travel.qmd` | `docs/travel.html` |
| `about.qmd` | `docs/about.html` |

注意：当前 `_quarto.yml` 的 `project.render` 只渲染根目录 `*.qmd`，根目录的 `*.md` 文件默认作为内部文档/草稿保留，不会生成公开页面。

## 5. 核心模块划分

### 5.1 站点配置 — `_quarto.yml`

控制：站点类型、输出目录、navbar、footer、favicon、repo actions、渲染入口、HTML 主题、TOC、静态资源复制。

**改 navbar / 标题 / footer / favicon / 新增页面导航 / 资源声明 → 改这里。**

### 5.2 页面内容层 — `*.qmd`

| 文件 | 职责 | 关键结构 |
|---|---|---|
| `index.qmd` | 首页 | solana hero + 紧凑论文卡片（按 Accepted / Under Review 分组） |
| `publications.qmd` | 论文列表 | 分组 + 结构化论文条目（title / authors / status / links） |
| `interests.qmd` | 研究方向 | 每个方向一个 `##` 区块 |
| `cv.qmd` | CV（PDF输出） | front matter `format: awesomecv-typst` + Typst 代码块 |
| `travel.qmd` | 旅行轨迹 | 按时间倒序排列的 `.travel-card` 卡片（图片 + 日期 + 地点 + 描述） |
| `about.qmd` | 占位页 | 极简，未导航 |

### 5.3 样式层

| 文件 | 职责 | 优先改动场景 |
|---|---|---|
| `styles.scss` | 全站主题基线（颜色变量、字体、hero、navbar） | 全站配色/字体/视觉基线 |
| `custom.css` | 后置覆盖补丁（hero 装饰层、卡片化、CV 页面布局） | 局部显示异常/定点修补 |

加载顺序：`cosmo → styles.scss → custom.css`，后者优先级最高。

### 5.4 CV 扩展 — `_extensions/kazuyanagimoto/awesomecv/`

`cv.qmd` 依赖此扩展生成 PDF。内容改动只需改 `cv.qmd`；模板/宏级别深改需进入扩展目录。

→ 详见 [`_extensions/kazuyanagimoto/awesomecv/readme_for_ai.md`](./_extensions/kazuyanagimoto/awesomecv/readme_for_ai.md)

### 5.5 静态资源

| 资源 | 用途 | 引用位置 |
|---|---|---|
| `image.png` | 首页主头像 | `index.qmd` front matter `image:` |
| `images/site-logo.png` | favicon | `_quarto.yml` |
| `rubrichub.pdf` | 论文附件 | 首页、论文页、CV、`_quarto.yml` resources |
| `paper.pdf` | 附件 | `_quarto.yml` resources |

## 6. 同步修改规则

某些内容在多处重复，非单点数据源：

| 变更类型 | 需同步检查的文件 |
|---|---|
| 论文信息更新 | `publications.qmd`, `index.qmd`, `cv.qmd`（完整列表 / 首页精简分组版 / CV 压缩版） |
| 个人简介/研究方向 | `index.qmd`, `interests.qmd`, `cv.qmd`, `email_template.txt` |
| 联系方式/链接 | `index.qmd`, `cv.qmd`, `email_template.txt` |
| 新增 PDF 附件 | 根目录放文件 + `_quarto.yml` resources + 页面引用 |

## 7. 需求定位速查表

| 需求 | 优先改 |
|---|---|
| 站点标题/navbar/footer/favicon | `_quarto.yml` |
| 首页文案/头像/链接/项目列表 | `index.qmd` |
| 首页 hero 区样式 | `custom.css` + `styles.scss` |
| 论文完整列表 | `publications.qmd`（+ 同步首页/CV） |
| 研究方向 | `interests.qmd` |
| CV 内容 | `cv.qmd` |
| CV 模板/宏 | `_extensions/kazuyanagimoto/awesomecv/` |
| 全站配色/字体 | `styles.scss` |
| 局部样式补丁 | `custom.css` |
| 旅行记录 | `travel.qmd`（新增卡片放最上面） + `images/travel/` 放照片 |
| 新增页面 | 新建 `*.qmd` + `_quarto.yml` navbar |
| 排除文件被渲染 | `_quarto.yml` |
| 新增可下载附件 | 根目录文件 + `_quarto.yml` resources + 页面链接 |

## 8. 坑点备忘

- `docs/` 是生成产物，不手改。
- `cv.qmd` 输出 PDF（不是 HTML）。
- `index.qmd` 使用 solana 横幅式 about 模板。
- 首页头像展示策略以 `styles.scss` 为准，当前使用保留原图比例的圆角矩形；`custom.css` 只处理装饰层和页面级补充样式。
- 根目录 `*.md` 文件（如 `AGENTS.md`、`readme_for_ai.md`、`test.md`）默认不会被渲染；如果未来想公开某个 Markdown 页面，需要改成 `*.qmd` 或调整 `_quarto.yml` 的 `project.render`。
- `webr/` 扩展存在但未使用。
- `search.json`（根目录）是历史文件，站点用 `docs/search.json`。

## 9. 子目录 AI 文档索引

| 路径 | 覆盖范围 |
|---|---|
| [`_extensions/kazuyanagimoto/awesomecv/readme_for_ai.md`](./_extensions/kazuyanagimoto/awesomecv/readme_for_ai.md) | CV PDF 模板扩展：文件职责、Typst 宏、修改入口 |

## 10. 维护流程

1. 读本文件，判断需求属于哪个模块。
2. 按速查表定位 1-3 个文件，直接修改。
3. 检查同步规则，确认多处内容一致。
4. `quarto render` 生成产物。
5. 如果本次修改改变了架构/文件职责/映射关系，更新本文件或对应子文档。
