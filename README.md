## Jiale Zhao — Personal Website

This repository powers my Quarto-based website at <https://jialeuuz.github.io>. 

### 快速开始

1. Fork 或复制本仓库。
2. 根据需要修改 `index.qmd`、`codes.qmd`、`publications.qmd`、`cv.qmd` 等文件中的个人信息。
3. 运行 `quarto render`（或使用 GitHub Actions 自动渲染）以生成 `docs/` 目录。
4. 将 `docs/` 目录部署到 GitHub Pages，即可上线个人主页。

### 本地预览

- 安装 Quarto CLI 后，在项目根目录运行：

  ```bash
  quarto preview
  ```

- 浏览器会自动打开本地地址；编辑 `*.qmd` 或 `_quarto.yml` 将触发热更新。

### 渲染命令说明

- 在项目根目录执行 `quarto render`（不带参数）会读取 `_quarto.yml` 并渲染站点中所有入口页面，结果输出到 `docs/`（或你在配置中指定的目录）。
- 如果只需渲染单个页面（例如 `cv.qmd`），可运行 `quarto render cv.qmd`；命令完成后仅该页面及依赖资源会被更新。

### 目录结构

```
├── index.qmd          # 首页
├── codes.qmd          # 代码与项目
├── publications.qmd   # 学术成果 / 工作论文
├── cv.qmd             # 简历页
├── _quarto.yml        # 网站配置
├── styles.scss        # 自定义样式
├── docs/              # 渲染后的静态页面
└── ...
```

### 致谢

- [Quarto](https://quarto.org/)
