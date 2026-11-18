# 可用的 HTML 片段

全部是本站已写好样式的普通 HTML。复制即用，不需要额外 CSS。
改完不需要重启任何东西——静态文件，刷新即可。

## 段落与导语

```html
<p class="lead">文章开头的一段，字号更大、颜色更亮。一篇文章只用一个。</p>

<p>普通段落。</p>
```

正文里的 `<strong>` 会变亮加粗，`<em>` 变亮，`<code>` 变成琥珀色的小胶囊。

## 标题

```html
<h2>小节标题</h2>      <!-- 自动带 § 角标，会进右侧目录 -->
<h3>更小的标题</h3>    <!-- 也会进目录，缩进一级 -->
```

h2 就是文章骨架，写 3–6 个。不要用 h4 及以下。

## 图片

**有真图**（放到 `assets/img/posts/<slug>/` 下）：

```html
<figure>
  <img src="/assets/img/posts/<slug>/classify.svg" alt="二分类是一条线，多分类是几条线把平面切开">
  <figcaption>二分类只需要一条边界，多分类要把空间切成 K 块</figcaption>
</figure>
```

图自带发丝边框、点击全屏放大。**alt 写清楚图里有什么**，不要写 "示意图"。

**没有真图**（笔记里只写了"这里放一张图"）：见 SKILL.md 的"占位符"一节，优先自己画 SVG，画不了再用这个占位框：

```html
<figure>
  <img src="/assets/img/posts/<slug>/placeholder-1.svg" alt="二分类与多分类的分类边界对比">
  <figcaption>二分类 vs 多分类（待补图）</figcaption>
</figure>
```

并生成 `placeholder-1.svg`（见 SKILL.md 里的模板），保证页面不会出现裂图。

**并排两张图**：

```html
<div class="img-row">
  <img src="…" alt="…">
  <img src="…" alt="…">
</div>
```

## 公式（MathJax）

行内用 `$…$`，独立成行用 `$$…$$`（会居中、独占一行、自动放大）。

```html
<p>样本属于第 $k$ 类的概率记作 $p_k$。</p>

<p>$$ p_k = \frac{e^{z_k}}{\sum_{j=1}^{K} e^{z_j}} $$</p>
```

多个条件并排，用 `\qquad` 或 `\quad` 隔开，不要写成两个 `$$` 块：

```html
<p>$$ \sum_{k=1}^{K} p_k = 1, \qquad 0 \le p_k \le 1 $$</p>
```

推导过程用 `aligned`：

```html
<p>$$ \begin{aligned} z_k &amp;= w_k^\top x + b_k \\ p_k &amp;= \frac{e^{z_k}}{\sum_j e^{z_j}} \end{aligned} $$</p>
```

注意：`<` 要写成 `&lt;`，`&` 要写成 `&amp;`（在 HTML 里是硬性要求，MathJax 也照此解析）。
**用了公式就必须在文末保留 MathJax 的两段 `<script>`**（模板里有），否则页面上会显示原始 LaTeX。

## 代码

```html
<p>一句话里的 <code>exp(z)</code> 用行内代码。</p>

<pre data-lang="python"><code>def softmax(z):
    e = np.exp(z - z.max())
    return e / e.sum()</code></pre>
```

`data-lang` 会显示在右上角，代码块悬停出现"复制"按钮。
代码里的 `<` `>` `&` 一样要转义。

## 表格

```html
<table>
  <thead><tr><th>模型</th><th>输出</th><th>损失</th></tr></thead>
  <tbody>
    <tr><td>二分类</td><td>一个 sigmoid</td><td>二元交叉熵</td></tr>
    <tr><td>多分类</td><td>K 个 softmax</td><td>交叉熵</td></tr>
  </tbody>
</table>
```

手机上表格会横向滚动，所以列不要超过 4 列。

## 提示块

```html
<div class="callout"><span class="ic">i</span><div>一个容易被忽略的细节：这里写两句。</div></div>
```

用于"直觉解释""常见坑""注意"。全文不超过 2 个，多了就变成警告框噪音。

## 引用

```html
<blockquote><p>一句想被记住的话。整篇文章最多一处。</p></blockquote>
```

适合放一句结论、一句定义、或者别人说的话（后面加 `<cite>出处</cite>`）。

## 列表

```html
<ul>
  <li>并列的三四条，每条一句话。</li>
  <li>不要写成一段长话拆行。</li>
</ul>
```

## 特效（可选，最多用 1–2 个）

```html
<span class="fx-scramble" data-text="PROBABILITY">PROBABILITY</span>   <!-- 文字解码，鼠标划过触发 -->
<button class="fx-magnet" type="button">磁吸按钮</button>              <!-- 只在桌面端 -->
<div class="fx-spot"><span class="s-t">一道很轻的光</span></div>        <!-- 局部聚光 -->
<div class="fx-shimmer">Light moves slowly.</div>                     <!-- 渐变动字 -->
```

技术文章里基本用不上，别为了炫技硬加——正文本身才是主角。

## 分隔与结尾

```html
<hr>
<p>最后一句话，或者一个"就写到这里"的收束。</p>
```

`<hr>` 中间会有一个 ✳ 标记，用来隔开"正文"和"后记"。
