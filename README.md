# 个人简历

纯静态的个人简历页面，零依赖（不需要 Node、Ruby，不加载任何 CDN）。

## 文件说明

本站源码直接放在仓库根目录，GitHub Pages 发布 `main` 分支的根目录，不需要 GitHub Actions。

```
.
├── index.html     页面结构 + 全部文字内容
├── style.css      样式（含手机端适配、打印样式）
├── Resume.pdf     简历占位文件，请替换成你自己的（文件名保持不变）
├── .nojekyll      告诉 GitHub Pages 不要用 Jekyll 处理
└── README.md      本文件
```

## 本地预览

直接双击 `index.html` 即可在浏览器打开。改完存盘刷新就行。

## 页面结构

**布局**：

- **顶部导航**（吸顶）：`概览 · 经历 · 技能 · 成果 · 荣誉 · 教育 · CV`，滚动时高亮当前区块并显示顶部进度条
- **左侧栏**（宽 300px，`position: sticky; top: 60px`）：头像、姓名、职位、三行方向标签、三个图标链接（学校 / 邮箱 / GitHub）
- **右侧栏**：内容区，随页面滚动
- 屏幕宽度小于 900px 时，侧栏回落成顶部区块，导航变成可横向滑动的一行

**图标**：用 Font Awesome 的原始 SVG 路径内联写在 HTML 里，不请求任何 CDN，断网也能显示。

**正文**（内容来自 `hyh-2026 Resume.md`）：

| 顺序 | 分区 | 锚点 | 格式说明 |
|---|---|---|---|
| 1 | 概览 Overview | `#about` | Hero 封面 `.hero`（姓名/职位/关键数字/CTA）+ 自我评价引文 `.quote` + 核心优势勾选列表 `.checks` |
| 2 | 工作经历 Experience | `#experience` | `.exp` 卡片（公司/职位/时间地点）+ 职责表 `.table` + 架构图 `.flow` + 关键成果表 |
| 3 | 技术能力 Skills | `#skills` | 分段进度条 `.meter`（`data-v` 控制亮几格）+ 工具链标签 `.tags` |
| 4 | 专利与论文 Publications | `#publications` | 专利表（名称/时间/状态/相关性）+ 论文 `.pub-list` |
| 5 | 获奖与荣誉 Honors | `#honors` | `[A1]` 编号 + 奖项 + 日期 + 角色/颁奖方 |
| 6 | 教育背景 Education | `#education` | 时间 + 学校 + 专业 + 学位 |

> 注意：`CV` 在顶部导航的最后一项，指向 `Resume.pdf`。
> 表格在 760px 以下自动堆叠成卡片（`th.t-key` 变成卡片标题，`td[data-label]` 显示字段名）。
> 分区标题的 `01–06` 编号由 CSS 计数器自动生成，增删分区不用手改序号。

## 视觉规范

- 配色：白底 + 单一强调色 `--accent: #2563eb`，文字分 `--text / --text-2 / --text-3 / --text-4` 四档，全部定义在 `style.css` 顶部 `:root`
- 字体：系统字体栈（零外部请求），数字与技术名词用 `--mono` 等宽栈
- 动效：滚动淡入由页面底部内联脚本控制，`prefers-reduced-motion: reduce` 时自动关闭
- 打印：`@media print` 会把强调色降为深灰、去掉卡片阴影，适合 `Ctrl + P` 存 PDF

## 内容维护说明

简历正文唯一来源是 `hyh-2026 Resume.md`，改内容时同步改 `index.html` 对应区块即可。常用位置：

| 要改的内容 | 在 index.html 里找 |
|---|---|
| 姓名 / 英文名 | Hero 里的 `<h1 class="hero-name">`，以及侧栏 `<p class="side-name">`（两处保持一致） |
| 左侧定位标签 | `<ul class="side-bio">` |
| 联系方式 | `<ul class="social-icons">`（邮箱、学校、GitHub） |
| 关键数字 | Hero 内的 `<ul class="hero-stats">` |
| 求职意向 / 自我评价 | `#about` 下的 `.hero-sub` 与 `.quote` |
| 一段工作经历 | `<article class="exp">`，整块复制/删除 |
| 核心职责 / 关键成果 / 代表项目 / 专利 | `<table class="table table-stack">`，新增行照抄 `<tr>`，首列用 `<th scope="row" class="t-key">` |
| 架构图节点 | `.flow` 内的 `.flow-node`、`.flow-bar` |
| 技能熟练度 | `.meter` 的 `data-v="1~5"`，同时改 `aria-label` |
| 工具链标签 | `.tags` 下的 `<li>` |
| 获奖 / 学历 | `.awards li` / `.edu li` |
| 更新时间 | 页脚 `.footer` |

**改进度条时**记得同步 `aria-label="熟练度 N 分，满分 5 分"`，否则读屏与打印会不一致。

## 换成真实头像

**头像** —— 现在是一个带渐变环和虚线圆的装饰性占位（`.avatar` 只用 CSS 画），换成真实照片：

```html
<img src="images/avatar.jpg" alt="头像" class="avatar" />
```

然后在 `style.css` 里补一句，让图片裁切填满（同时关掉装饰用的两道伪元素）：

```css
img.avatar { object-fit: cover; object-position: top; border-radius: 50%; }
img.avatar::before, img.avatar::after { display: none; }
```

## 替换简历

`Resume.pdf` 现在是占位文件。用你自己的 PDF 覆盖它，**文件名保持 `Resume.pdf`**（大小写敏感），
导航栏的 `CV` 按钮就会指向新文件。

## 部署方式

- 仓库名必须是 `用户名.github.io`，且为 **Public**
- Settings → Pages → **Source 选 `Deploy from a branch`**，分支 `main`，目录 `/ (root)`
- 每次 `git push` 到 `main`，Pages 会自动重新构建，约 1 分钟生效

日常更新：

```powershell
git add -A
git commit -m "更新内容"
git push
```

注意：所有内容改动都直接改仓库根目录里的文件。

## 小技巧

`Ctrl + P` 可直接打印成 PDF，打印样式已隐藏导航栏和头像，适合临时当简历用。
