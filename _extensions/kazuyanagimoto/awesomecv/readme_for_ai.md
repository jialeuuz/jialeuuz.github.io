# readme_for_ai — awesomecv Typst 扩展

> CV PDF 模板扩展。`cv.qmd` 通过 `format: awesomecv-typst` 使用此扩展生成 PDF。

## 在项目中的位置

```
cv.qmd (内容 + front matter)
  ↓ format: awesomecv-typst
_extensions/kazuyanagimoto/awesomecv/
  ├── _extension.yml       # 扩展声明（版本、contributes）
  ├── typst-show.typ       # Pandoc 模板变量 → resume() 函数调用的桥接层
  ├── typst-template.typ   # 核心：所有 Typst 宏定义 + PDF 版式
  └── input-yaml.lua       # Lua shortcode：从 YAML 文件生成 resume-entry
  ↓ quarto render
docs/cv.pdf
```

## 文件职责

### `_extension.yml`
- 声明扩展名称、版本 (`0.3.0`)、Quarto 最低版本要求 (`>=1.7.33`)。
- 注册 `typst-template.typ` 和 `typst-show.typ` 为 template-partials，`input-yaml.lua` 为 shortcode。
- **一般不需要改**，除非升级扩展版本或新增 partial。

### `typst-show.typ`
- 桥接层：将 Quarto front matter 变量（`$author.firstname$`、`$brand.color.primary$` 等）映射为 `resume()` 函数参数。
- 支持两套字体/颜色来源：`style.*`（旧）和 `brand.*`（新，`cv.qmd` 当前使用 brand）。
- **修改场景**：需要从 front matter 传入新参数时改这里。

### `typst-template.typ`（核心文件）
- 定义所有 PDF 版式和宏：
  - **`resume()`**：顶层文档模板，控制页面尺寸 (A4)、边距、字体、颜色、heading 样式。
  - **`resume-entry(title, location, date, description)`**：带左右对齐的条目宏（用于 Education、Experience）。
  - **`resume-item(body)`**：列表内容宏（用于 Publications、Ongoing Work 等纯列表区块）。
  - **`create-header()`**：PDF 顶部 header（姓名、职位、联系方式图标）。
  - 辅助函数：`justified-header`、`secondary-justified-header`、`parse_icon_string`。
- **颜色常量**：`color-darknight`、`color-darkgray` 等在文件顶部定义。
- **依赖**：`@preview/fontawesome:0.5.0`（图标）。

### `input-yaml.lua`
- Lua shortcode `{{< yaml file.yml >}}`，从 YAML 文件读取条目并生成 `resume-entry` + `resume-item` Typst 代码。
- **当前 `cv.qmd` 未使用此 shortcode**（直接在 QMD 中写 Typst 代码块），但扩展保留了这个能力。

## 修改导航

| 需求 | 改哪里 |
|---|---|
| CV 文字内容（经历、论文、简介） | `cv.qmd` |
| CV 品牌色 / 字体 | `cv.qmd` front matter `brand:` |
| PDF 页面尺寸 / 边距 | `typst-template.typ` → `resume()` → `set page(...)` |
| 条目布局（左右对齐比例、间距） | `typst-template.typ` → `resume-entry()` / `justified-header()` |
| Header 样式（姓名大小、联系方式排列） | `typst-template.typ` → `create-header*()` 系列函数 |
| 新增 front matter 参数传入 | `typst-show.typ` |
| 从 YAML 文件批量生成条目 | 使用 `input-yaml.lua` shortcode |

## `cv.qmd` 中使用的宏

在 `cv.qmd` 的 ` ```{=typst}` 代码块中：
- `#resume-entry(title: [...], location: [...], date: [...], description: [...])` — 结构化条目
- `#resume-item[...]` — 纯列表内容
- `#strong[...]` — 加粗（Typst 内置）
