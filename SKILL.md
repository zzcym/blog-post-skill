---
name: blog-post
description: 把一段粗糙的笔记写成 zzcym 博客（https://zzcym.github.io）的成品 HTML 文章：纠错、补全、按站点样式排版，把"这里放张图""这里放个公式"这类占位变成真的图和 MathJax 公式，最后重建索引并自检。当用户给出笔记/草稿/要点、说"写成博客""整理成文章""发一篇"，或笔记里出现占位括号时使用。
whenToUse: 用户给出一段不完整的笔记、口述、要点或截图说明，希望变成一篇可以发布的博客文章时。
metadata:
  blog_root: E:\DSH\zzcym.github.io
  site_url: https://zzcym.github.io
---

# 把笔记写成一篇博客

## 交付物

- 一个文件：`posts/<slug>.html`（从本 skill 的 `templates/post.html` 复制）
- 重新生成的 `posts.json` / `feed.xml` / `sitemap.xml`（脚本产物，不要手改）
- 必要时新增的图片：`assets/img/posts/<slug>/*.svg`
- 一段交付说明（见最后一节）

**默认不 push。** 只有用户说了"发""推上去""发布"才 `git push`。

## 0. 先定位博客仓库

默认在 `E:\DSH\zzcym.github.io`。先确认它还在：

```bash
BLOG="E:/DSH/zzcym.github.io"
ls "$BLOG/posts.json" "$BLOG/tools/build-index.mjs" "$BLOG/assets/js/mathjax/tex-svg.js"
```

不在就找 `posts.json` + `tools/build-index.mjs` + `assets/css/style.css` 同时存在的目录；
找不到就问用户，不要另起一个新站点。

## 1. 读笔记，先在脑子里做四件事

不要急着写 HTML。先把笔记拆开：

1. **论点**：这段笔记到底想让读者"啊"一下的是哪一句？（通常只有一个）
2. **要纠的错**：术语、逻辑跳跃、符号不一致、事实不精确——对照 `references/style.md` 第三节。
3. **缺的东西**：读者会在哪一步卡住？需要补一个例子 / 一个"为什么" / 一个坑。
4. **占位符**：把每个 `（这里放……）` 单独列出来，标明它是图、公式、表格还是代码。

拆完之后在心里排一个结构（问题 → 直觉 → 形式化 → 细节 → 收束），再动手。

## 2. 定标题、摘要、标签、文件名

- 标题：两段式或一句完整的话，≤30 个汉字。见 `references/style.md` 第六节。
- 摘要（`post:excerpt`）：40–70 字，说清"这篇讲了什么"，不是标题的重复。
- 标签：1–2 个，优先复用已有标签（`站点 指南 前端 特效 项目 地图 设计 界面 机器学习 数学 思考`）。
- slug：小写英文短词，`softmax` / `why-softmax`，不要 `post-20261119`。
- 日期：默认今天（`date +%F`）。

## 3. 生成文件

```bash
cp "$HOME/.dsh/skills/blog-post/templates/post.html" "$BLOG/posts/<slug>.html"
```

然后把模板里这些占位全部替换掉（**一个都不能剩**）：
`__TITLE__` `__EXCERPT__` `__DATE__` `__DATE_DOT__`（形如 `2026.11.19`）`__TAGS__`（英文逗号分隔）`__TAGS_HTML__`（每标签一个 `<span class="tag">`）`__SLUG__` `__BODY__`。

正文片段全部从 `references/components.md` 里挑，不要自己发明 class 或 CSS。

**有公式就保留末尾 MathJax 的两段 `<script>`；没有公式就删掉。**

## 4. 写正文

按 `references/style.md` 的形状和语气写。几条硬要求：

- 开头一个 `<p class="lead">`，一篇文章只用一个。
- 3–6 个 `<h2>`，每个下面 2–4 段。h3 只在真需要时用。
- 一篇文章最多 2 个 `callout`，1 个 `blockquote`，1 个 `<hr>`。
- 每段 2–5 行，中文与英文/数字之间加半角空格。
- 特效组件（`fx-*`）技术文里基本不用。
- 不确定的事实写成 `<!-- 待确认：… -->`，并在交付说明里列出来。**不要编。**

篇幅：笔记只有三句话，也写到 600 字左右（靠补直觉和细节），不要到 2500。

## 5. 处理占位符

笔记里的占位通常长这样：`（这里放一张图…）`、`（这里放两个公式…）`、`（后面放 softmax 公式）`。
它们不是"随便插一个东西"，而是作者已经想好了那里该有什么。逐个落实：

### 图片

**先判断能不能自己画。** 示意图、流程图、对比图、坐标系、柱状对比——这些用 SVG 二十行就能画出来，而且比一张临时找的图更统一。

- 能画：写一个 SVG 存到 `assets/img/posts/<slug>/01-xxx.svg`，然后在正文里 `<figure>` 引用它。
- 画不了（真实截图、照片、复杂插图）：用占位框，并在正文里保留 `<!-- TODO 图：… -->`。

SVG 要遵守站点的视觉：深色背景透明、线条 `#f1ece2` 低透明度、强调色 `#ff5a36`、字体用 `font-family="monospace"` 或系统无衬线、尺寸 `viewBox="0 0 800 300"` 左右、**不要**写死背景色（页面是深色的）。

占位框 SVG 模板（复制后改文字）：

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 800 320" width="800" height="320">
  <rect x="1" y="1" width="798" height="318" fill="none" stroke="#f1ece2" stroke-opacity=".18" stroke-dasharray="6 8"/>
  <text x="400" y="150" text-anchor="middle" fill="#8d8779" font-family="monospace" font-size="13" letter-spacing="2">IMAGE PLACEHOLDER</text>
  <text x="400" y="180" text-anchor="middle" fill="#c9c3b6" font-family="sans-serif" font-size="16">这里要一张：二分类与多分类的边界对比</text>
  <text x="400" y="210" text-anchor="middle" fill="#57534c" font-family="monospace" font-size="11">TODO</text>
</svg>
```

figcaption 写"图里有什么"，不要写"示意图"。

### 公式

直接写成 LaTeX，不要用文字描述"概率之和为一"：

- 行内：`$p_k$`
- 独立：`<p>$$ … $$</p>`
- 几个条件并排：一个 `$$` 里用 `\qquad` 隔开
- 推导：`\begin{aligned} … \end{aligned}`
- HTML 里 `<` `>` `&` 必须转义成 `&lt;` `&gt;` `&amp;`

笔记说"这里放两个公式，一个是……另一个是……"→ 通常合成**一个** `$$` 块并排写，比两个分开的块更好看。

### 表格 / 代码

笔记描述"对比一下 A 和 B"→ `<table>`；描述"大概是这样的代码"→ `<pre data-lang="python">`，代码要能真跑。

## 6. 重建索引

```bash
cd "$BLOG" && node tools/build-index.mjs
```

输出里应该能看到新文章。看不到 = 文件名或 `<meta name="post:*">` 有问题，先修。

## 7. 自检（必须做，不要跳过）

**静态检查**：

```bash
grep -n "__\|TODO\|待确认" "$BLOG/posts/<slug>.html"   # 占位符必须清空（TODO 图除外）
```

**目视检查**（本地起服务 + 无头浏览器截图）：

```bash
cd "$BLOG" && node tools/serve.mjs 8765 &
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu \
  --hide-scrollbars --window-size=1440,3200 --virtual-time-budget=9000 \
  --screenshot="$BLOG/.tmp/preview.png" "http://127.0.0.1:8765/posts/<slug>.html?static=1"
```

`?static=1` 会关掉所有动效与入场动画，保证截图是最终样子。看四件事：

1. 公式渲染了没有（没渲染就是漏了 MathJax 的 `<script>`）
2. 图裂没裂（路径以 `/` 开头，大小写一致）
3. 目录（右侧）有没有空、有没有超长条目
4. 第一屏是不是太挤：标题、摘要、导语之间有没有呼吸

窗口宽度有下限（约 500px），想看手机版就把页面套进一个 390px 宽的 iframe 再截图。

**内容检查**：把正文当读者读一遍——
- 有没有一句话是从笔记里照抄、但对着上下文已经不成立的？
- 有没有"这里""上面"指向不明？
- 公式里的符号和正文里的叫法是不是同一套？
- 有没有哪一段删掉之后文章照样通顺？（有的话，删掉它）

## 8. 交付说明

最后给用户的回复里，用短段落写清四件事：

1. 写了哪篇、路径、标题；发布了没有（没发就给一条 `git push` 命令）。
2. **改了哪些错**（1–3 条，说清原来的说法为什么不对）。
3. **补了什么、为什么补**（1–3 条）。
4. **还缺什么**：待补的图、待确认的事实、需要用户自己填的地方。

不要贴整篇 HTML；用户已经有文件了。最多贴两句最有代表性的改动前后对比。

## 常见翻车

| 现象 | 原因 |
|---|---|
| 页面上直接显示 `$p_k$` 原文 | 忘了保留 MathJax 的两段 `<script>`（模板末尾） |
| 行内公式自己独占一行 | `style.css` 第 15 节那条 `mjx-container { display: inline-block }` 被覆盖或删掉了 |
| 新文章在列表页看不到 | 没跑 `node tools/build-index.mjs`，或 `<meta name="post:*">` 拼错/漏了一行 |
| 排序不对 | `post:date` 不是 `YYYY-MM-DD` |
| 图裂 | 路径没以 `/` 开头，或大小写不对（GitHub Pages 区分大小写） |
| 右侧目录是空的 | 正文里没有 `<h2>`/`<h3>`，或标题写到 `.prose` 外面去了 |
| 手机上公式溢出 | 单个公式太长，拆成两行，或用 `\begin{aligned}` 换行 |
| 页面白屏 | 模板里的 `__TOKEN__` 没替换干净 |

截图用的 `.tmp/` 记得删掉，不要提交进仓库。
