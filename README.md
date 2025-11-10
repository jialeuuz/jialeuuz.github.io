## Jiale Zhao — Personal Website

This repository powers my Quarto-based website at <https://jialeuuz.github.io>. 

### 快速开始

1. Fork 或复制本仓库。
2. 根据需要修改 `index.qmd`、`codes.qmd`、`publications.qmd`、`cv.qmd` 等文件中的个人信息。
3. 运行 `quarto render`（或使用 GitHub Actions 自动渲染）以生成 `docs/` 目录。
4. 将 `docs/` 目录部署到 GitHub Pages，即可上线个人主页。
5. 若需导出 PDF 简历，可运行 `quarto render cv.qmd --to pdf`。

### 目录结构

```
├── index.qmd          # 首页
├── codes.qmd          # 代码与项目
├── publications.qmd   # 学术成果 / 工作论文
├── cv.qmd             # 简历页（Quarto 版本，可导出 PDF）
├── _quarto.yml        # 网站配置
├── styles.scss        # 自定义样式
├── docs/              # 渲染后的静态页面
└── ...
```

### 致谢

- [Quarto](https://quarto.org/)
