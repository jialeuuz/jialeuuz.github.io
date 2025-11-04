
## 如何生成个人主页

我的 [个人主页](https://lianxhcn.github.io/) 是基于 [Chi Zhang](https://chizapoth.github.io/) ([github](https://github.com/chizapoth/chizapoth.github.io)) 的个人主页修改而来的。 

你可以 Fork 本仓库，然后酌情修改你的个人信息，编译后即可生成你的个人主页。这种方法无需注册域名，也不用学习 HTML 和 CSS 等前端知识，直接使用 GitHub Pages 即可完成个人主页的搭建。

具体制作过程参见：

- 连小白, 2025, [50 分钟搞定个人主页：Fork 模板 + GitHub Pages + Quarto 完整教程](https://www.lianxh.cn/details/1644.html).

本仓库的使用方法：

- 你可以点击本页右上方的 ⭐**Star** 按钮来收藏本仓库，方便日后查阅；
- 也可以点击 **Use this template** 按钮来快速 Fork 本仓库。
- 你制作个人主页期间有任何问题，可以在 [Discussions](https://github.com/lianyujun/lianyujun.github.io/discussions) 提问和讨论。

--- 

<center>

lianyujun.github.io

基本文件：
├── readme.md                # 仓库说明
├── index.qmd                # 主页内容
├── _quarto.yml              # Quarto 配置文件（导航栏、主题、输出路径等）
├── styles.scss              # 自定义样式（可选，可以自行修改）
├── images/                  # 文件夹：存放头像和网页 logo 等图片

个人信息页面：
├── chinese.qmd              # 中文内容页面
├── codes.qmd                # 代码资源页面
├── publications.qmd         # 学术成果页面

生成的网站：
├── docs/                    # 文件夹：渲染生成后的网页目录，用于部署到 GitHub Pages

其他配置信息
├── site_libs/               # Quarto 构建生成的支持库（可忽略）
├── _extensions/             # Quarto 扩展插件目录（如 webr，可以忽略）
├── license                  # 版权声明
└── _freeze/                 # Quarto 自动管理的中间结果缓存（可忽略）
</center>