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

- **顶部导航**（吸顶）：`概览 · 教育 · 项目 · 荣誉 · 成果 · CV`
- **左侧栏**（宽 292px，`position: sticky; top: 56px`）：头像、姓名、三行方向标签、三个图标链接（学校 / 邮箱 / GitHub）
- **右侧栏**：内容区，随页面滚动
- 屏幕宽度小于 900px 时，侧栏回落成顶部区块，导航变成可横向滑动的一行

**图标**：用 Font Awesome 的原始 SVG 路径内联写在 HTML 里，不请求任何 CDN，断网也能显示。

**正文**：

| 顺序 | 分区 | 锚点 | 格式说明 |
|---|---|---|---|
| 1 | 个人简介 Overview | `#about` | 3 段自述 |
| 2 | 教育背景 Education | `#education` | 时间 + 学校 + 专业 + 学位 |
| 3 | 项目经历 Projects | `#projects` | 名称 + 时间角色 + 描述 + 技术栈 |
| 4 | 荣誉与奖项 Honors and Awards | `#honors` | `[A1]` 编号 + 级别 + 奖项 + 日期 + 颁发单位 |
| 5 | 论文与专利 Publications & Patents | `#publications` | 论文一行式列表 + 专利一行式列表 |

> 注意：`CV` 在顶部导航的最后一项，指向 `Resume.pdf`。

## 要替换的内容清单

在 `index.html` 里搜索这些关键词：

| 搜索 | 说明 |
|---|---|
| `hhh` | 姓名占位（左侧栏 1 处、个人简介 1 处、论文作者 2 处），换成你的真名 |
| `Yaohua.Hu - Homepage` | 浏览器标签标题与分享标题 |
| `yaohua0506@163.com` | 邮箱（侧栏，共 2 处：链接地址 + 显示文字） |
| `某某教授` | 导师姓名 |
| `https://www.hitwh.edu.cn/` | 学校官网链接（侧栏） |
| `https://github.com/yaohua0506` | GitHub 地址 |
| `多模态学习` | 左侧栏方向标签 |
| `<article class="proj">` | 一个项目一块，复制/删除增减 |
| `<li><strong>[A1]</strong>` | 一条荣誉 |
| `<li><em>2025.09` | 一段学历 |
| `arXiv:2512.xxxxx` | 论文编号 |
| `<li><strong>[P1]</strong>` | 一条专利 |

## 换成真实头像

**头像** —— 现在是一个空白圆形占位（`.avatar` 只设了底色和边框），换成真实照片：

```html
<img src="images/avatar.jpg" alt="头像" class="avatar" />
```

然后在 `style.css` 末尾补一句，让图片裁切填满：

```css
img.avatar { object-fit: cover; object-position: top; border-radius: 50%; }
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
