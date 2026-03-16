# readme_for_ai

本文件的目标是：后续 AI 只要先读这一份，就能快速理解这个仓库的整体架构、页面与样式的分层关系、各模块对应的代码路径，以及“一个需求通常应该改哪些文件”，而不需要重新大范围扫仓库。

## 1. 项目一句话

这是一个基于 Quarto 的个人学术主页仓库，部署目标是 GitHub Pages。

真实渲染链路如下：

`源文件（*.qmd / 少量 *.md / 静态资源） -> _quarto.yml -> quarto render -> docs/ -> GitHub Pages`

最重要的原则：

- 优先改源文件，不手改 `docs/` 下的生成产物。
- 页面内容主要在 `*.qmd`。
- 全站配置在 `_quarto.yml`。
- 全站主题基线在 `styles.scss`，局部覆盖与页面修补在 `custom.css`。
- `docs/` 是部署产物，不是事实上的源码。

## 2. 核心架构总览

### 2.1 入口配置

- `_quarto.yml`
  - Quarto 站点总入口。
  - 定义站点类型、输出目录 `docs/`、navbar、footer、favicon、repo actions、HTML 主题、TOC、额外资源复制规则。
  - 如果需求涉及“站点级行为”，优先看这里。

### 2.2 页面源码层

- `index.qmd`
  - 首页。
  - 使用 Quarto `about: template: trestles` 生成 hero/profile 区块。
  - 同时承载个人简介、背景、精选论文、ongoing work、selected projects、resubmission manuscripts。
  - 还包含一个内联 HTML 区块 `#left-research` 和一段内联脚本，用来把额外内容插入到 about links 下方。

- `publications.qmd`
  - 论文列表页。
  - 当前结构很简单，核心内容集中在 `## Under Review` 下。

- `interests.qmd`
  - Research Interests 页面。
  - 每个研究方向是一个单独二级标题区块，适合按主题增删。

- `cv.qmd`
  - CV 源文件。
  - 不走普通 HTML 页面，而是通过 `awesomecv-typst` 格式生成 PDF。
  - 输出目标是 `docs/cv.pdf`，navbar 里直接链接 `cv.pdf`。

- `about.qmd`
  - 一个非常简短的占位页，目前未在 navbar 中暴露。
  - 仍会被 Quarto 渲染为 `docs/about.html`。

### 2.3 样式层

- `styles.scss`
  - 全站主题基线。
  - 负责颜色变量、字体、正文/标题样式、链接样式、导航栏样式、首页 hero 的基础视觉。
  - 在 Quarto 配置中作为 theme 链的一部分加载：`[cosmo, styles.scss]`。

- `custom.css`
  - 后置覆盖层。
  - 当前主要用于：
    - 修正首页头像在 trestles 模板中的裁剪/背景问题。
    - 设置首页左栏额外内容 `.about-left-extra`。
    - 调整 CV 页面 HTML 容器布局相关样式。
  - 如果某个视觉问题“全站主题里有，但只想定点修补”，优先看这里。

### 2.4 静态资源层

- `images/site-logo.png`
  - 当前 favicon。

- `image.png`
  - 首页 `index.qmd` front matter 中声明的主头像/主图。

- `images/profile.png`
  - 当前仓库内存在，但在现有源码里未发现被引用；大概率是备用头像资源。

- `rubrichub.pdf`
  - 被首页、论文页、CV 页面引用。
  - 也是 `_quarto.yml` 中声明的资源之一，渲染时会复制到 `docs/`。

- `paper.pdf`
  - 被 `_quarto.yml` 声明为资源，会复制到 `docs/`。
  - 在当前主页面源码中未发现直接引用，属于可用静态附件。

- `robots.txt`
  - 搜索引擎抓取配置，渲染时会复制到 `docs/robots.txt`。

### 2.5 扩展层

- `_extensions/kazuyanagimoto/awesomecv/`
  - `cv.qmd` 所依赖的 Quarto 扩展。
  - 如果 CV 的 PDF 版式或 Typst 模板能力需要深改，优先从这里查：
    - `_extensions/kazuyanagimoto/awesomecv/_extension.yml`
    - `_extensions/kazuyanagimoto/awesomecv/typst-template.typ`
    - `_extensions/kazuyanagimoto/awesomecv/typst-show.typ`
    - `_extensions/kazuyanagimoto/awesomecv/input-yaml.lua`

- `webr/`
  - 仓库中存在 webR 扩展。
  - 当前页面源码没有发现实际使用该扩展的页面或代码块。
  - 现阶段可视为“预留能力”，除非需求明确要加 webR 交互内容，否则通常不用动。

### 2.6 部署产物层

- `docs/`
  - GitHub Pages 实际服务目录。
  - 所有 HTML/PDF/搜索索引/站点 JS/CSS 库都在这里。
  - 原则上不手工编辑，统一由 `quarto render` 生成。

## 3. 源文件到输出文件的映射

当前主映射关系：

- `index.qmd` -> `docs/index.html`
- `publications.qmd` -> `docs/publications.html`
- `interests.qmd` -> `docs/interests.html`
- `about.qmd` -> `docs/about.html`
- `cv.qmd` -> `docs/cv.pdf`
- `AGENTS.md` -> `docs/AGENTS.html`
- `test.md` -> `docs/test.html`

额外注意：

- 根目录下并非只有 `*.qmd` 会参与渲染，部分 `*.md` 也会进入站点输出。
- `README.md` 当前没有被渲染到 `docs/`，但 `AGENTS.md` 和 `test.md` 会。
- 这意味着新建在根目录的 `readme_for_ai.md` 在未来执行完整 `quarto render` 时，大概率也会被渲染为公开页面，通常会对应 `docs/readme_for_ai.html`。
- 如果未来不希望它出现在站点产物中，需要在 `_quarto.yml` 里显式排除。

## 4. 关键文件内部结构

### 4.1 `_quarto.yml`

当前主要负责：

- `project.type: website`
- `project.output-dir: docs`
- `project.resources`
  - 当前包含 `paper.pdf` 和 `rubrichub.pdf`
- `website.title`
- `website.site-url`
- `website.favicon`
- `website.repo-url`
- `website.repo-actions`
- `website.page-footer`
- `website.navbar`
- `format.html.theme`
- `format.html.css`
- `format.html.toc`

因此，凡是下面这类需求，优先定位 `_quarto.yml`：

- 改 navbar 项目
- 改网站标题
- 改 footer
- 改 repo edit / issue 按钮
- 改 favicon
- 改输出目录
- 增加需要被复制到 `docs/` 的静态资源
- 调整全站 HTML 格式设置（主题、TOC 等）

### 4.2 `index.qmd`

关键结构：

- front matter
  - `title`
  - `image`
  - `about`
    - `template: trestles`
    - `image-width`
    - `image-shape`
    - `links`
- 正文区块
  - `## Background`
  - `## Publications`
  - `## Ongoing Work`
  - `## Selected Projects`
  - `## Manuscripts for Resubmission`
- 内联 HTML
  - `<div id="left-research" class="about-left-extra">`
- 内联脚本
  - DOMContentLoaded 后把 `#left-research` 挂到 `.about-links` 后面

因此，凡是下面这类需求，优先定位 `index.qmd`：

- 改首页自我介绍
- 改首页 hero 区链接（邮箱/GitHub/CV）
- 改首页头像引用文件
- 改首页背景/经历/ongoing work/selected projects/manuscripts 文案
- 改首页左栏 “Research Experience” 内容
- 改首页插入逻辑或 about 区块结构

### 4.3 `publications.qmd`

关键结构很集中：

- front matter: `title`
- 主区块：`## Under Review`

适合处理：

- 增删正式论文条目
- 调整论文描述长文案
- 做完整 publication list 的维护

注意同步：

- 如果一篇论文同时出现在首页摘要或 CV 里，通常还需要同步改：
  - `index.qmd`
  - `cv.qmd`

### 4.4 `interests.qmd`

当前每个方向一个二级标题：

- `## Human–LLM Interaction under Underspecification`
- `## Agent-based LLMs and Multi-step Reasoning`
- `## Data Flywheels and Self-improving Systems`
- `## MindGPTo: End-to-end Multimodal Interaction`
- `## Interpretability, Safety, and Controllability`
- `## Evaluation and Benchmarks (“Realistic, Even if ‘Dumb’”)`

适合处理：

- 新增/删除/重写研究方向
- 调整每个方向下的 Goal / Methods / Evaluation / Data 等子段落

### 4.5 `cv.qmd`

关键结构：

- front matter
  - `title`
  - `author`
  - `format: awesomecv-typst`
  - `brand.typography`
  - `brand.color`
- 内容区块
  - `## Personal Profile`
  - `## Education`
  - `## Publications (Under Review)`
  - `## Ongoing Work`
  - `## Experience`
  - `## Manuscripts for Resubmission`
- 多个 ` ```{=typst}` 代码块
  - 使用 Typst 宏输出条目与列表

适合处理：

- 改 CV 文案
- 改联系方式
- 改 CV 中的论文/经历条目
- 改 PDF 主题色、字体

深度改版时的定位规则：

- 只是内容变化 -> 改 `cv.qmd`
- 只是品牌色/字体 -> 优先改 `cv.qmd` front matter 的 `brand`
- 需要改 PDF 结构模板/宏行为 -> 查 `_extensions/kazuyanagimoto/awesomecv/`

### 4.6 `styles.scss`

当前职责分两层：

- defaults 层
  - 颜色变量
  - 字体变量
  - Bootstrap/Quarto 主题变量
- rules 层
  - 正文字体与行距
  - 标题样式
  - 链接样式
  - 首页 trestles hero 基础视觉
  - 导航栏样式

适合处理：

- 改全站主色
- 改标题/正文/链接基线风格
- 改 navbar 默认风格
- 改首页 hero 的主视觉基线

### 4.7 `custom.css`

这是当前最明显的“补丁层”文件，主要处理：

- 首页头像的显示策略
  - 取消/覆盖 trestles 默认圆形裁剪副作用
  - 去除背景伪元素、阴影与灰底
- 首页左侧附加内容 `.about-left-extra`
- CV 相关样式
  - `body.page-cv`
  - `.cv`
  - `.cv-section`
  - `.cv-item`

适合处理：

- 某个页面局部显示异常
- 不想动主题基线，只想做后置覆盖
- 修 trestles 模板与自定义图片之间的冲突

## 5. 需求定位速查表

下面这张表是后续最常用的部分。

| 需求类型 | 优先修改文件 | 说明 |
| --- | --- | --- |
| 改站点标题、navbar、footer、favicon、repo actions、TOC | `_quarto.yml` | 站点级配置都在这里 |
| 改首页文案、主页链接、主页头像引用、项目列表 | `index.qmd` | 首页内容主入口 |
| 改首页左栏额外 Research Experience | `index.qmd`, `custom.css` | 内容在 `index.qmd`，样式在 `custom.css` |
| 改论文完整列表 | `publications.qmd` | 如首页/CV也展示同一论文，需要同步 |
| 改首页论文摘要区 | `index.qmd` | 这里只放首页版本 |
| 改研究方向页 | `interests.qmd` | 每个方向是一个独立二级标题 |
| 改 CV 内容 | `cv.qmd` | 输出是 PDF，不是 HTML |
| 改 CV PDF 的模板结构 | `_extensions/kazuyanagimoto/awesomecv/` | 模板/宏级别修改 |
| 改全站配色、字体、hero 基线样式 | `styles.scss` | 全站主题层 |
| 改局部样式补丁、头像裁剪、CV 局部 CSS | `custom.css` | 后置覆盖层 |
| 更换首页头像图片 | `image.png` 或 `index.qmd` 中的 `image:` | 如果换文件名，记得同步 front matter |
| 更换 favicon | `images/site-logo.png`, `_quarto.yml` | 资源文件和引用都要看 |
| 新增可下载 PDF/附件 | 根目录资源文件, `_quarto.yml`, 对应页面源码 | `project.resources` 负责复制到 `docs/` |
| 新增页面并加入导航 | 新建 `*.qmd`, 修改 `_quarto.yml` | 然后执行 render |
| 不想让某个根目录文档被公开渲染 | `_quarto.yml` | 需要显式排除渲染 |
| 改抓取/索引相关基础设置 | `robots.txt`, `_quarto.yml` | `search.json` 不应手动作为主编辑入口 |

## 6. 同步修改规则

某些内容在多个页面重复出现，不是单点数据源。常见同步规则如下：

- 论文信息更新
  - 通常至少检查：
    - `publications.qmd`
    - `index.qmd`
    - `cv.qmd`
  - 如果有对应 PDF，也要确认：
    - `rubrichub.pdf`
    - `_quarto.yml` 的 `project.resources`

- 个人简介/研究方向变化
  - 通常至少检查：
    - `index.qmd`
    - `interests.qmd`
    - `cv.qmd`
    - `email_template.txt`（如果对外联络模板也需要同步）

- 联系方式或主页链接变化
  - 通常至少检查：
    - `index.qmd`
    - `cv.qmd`
    - `email_template.txt`

## 7. 当前仓库里的“隐式行为”和坑点

这些是后续 AI 很容易忽略、但实际上非常关键的点：

- `docs/` 中的内容是生成产物，不应作为主要编辑目标。
- `cv.qmd` 产出的是 `cv.pdf`，不是常规 `cv.html`。
- `index.qmd` 不是纯 Markdown 页面，里面带有内联 HTML 和内联脚本。
- 首页头像相关样式同时在 `styles.scss` 和 `custom.css` 里出现；如果出现显示冲突，最终以后置的 `custom.css` 为准。
- 根目录 `AGENTS.md` 和 `test.md` 当前会被渲染进站点，即使它们不在 navbar 中。
- `about.qmd` 当前未在导航中显示，但仍然是有效页面。
- 新建根目录 `readme_for_ai.md` 后，未来完整 render 时大概率也会公开生成一个 HTML 页面；如果不想公开，需要额外改 `_quarto.yml`。
- `webr/` 扩展当前存在但未被实际页面使用，除非明确做 webR 功能，否则可以忽略。
- 根目录 `search.json` 看起来像手工或历史文件；当前站点实际使用的是渲染产物 `docs/search.json`，通常不应把根目录这个文件当成主修改目标。
- `tmp_gallery.html`、`notes.txt`、`email_template.txt` 更像辅助/草稿/运营材料，不属于站点主渲染链路。

## 8. 推荐的后续维护流程

如果后续有新需求，建议按下面顺序处理：

1. 先读本文件，判断需求属于“站点配置 / 页面内容 / 样式 / 静态资源 / CV 模板”哪一类。
2. 只打开对应的 1 到 3 个核心文件，不要先全仓库扫描。
3. 修改源文件，不手改 `docs/`。
4. 如果改的是站点对外可见内容，执行 `quarto render` 或按页面定向 render。
5. 如果本次修改改变了架构、页面职责、文件映射或同步规则，顺手更新本文件。

## 9. 快速结论

对后续 AI 来说，这个仓库最重要的定位规则可以压缩成下面几句：

- 站点级改动先看 `_quarto.yml`。
- 首页相关需求先看 `index.qmd`。
- 论文页看 `publications.qmd`。
- 研究方向页看 `interests.qmd`。
- CV 内容看 `cv.qmd`，CV 模板深改看 `_extensions/kazuyanagimoto/awesomecv/`。
- 全站视觉基线看 `styles.scss`，页面级补丁看 `custom.css`。
- 静态部署产物在 `docs/`，不要直接改。

如果后续需求会改变上述任一映射关系，必须同步更新本文件。
