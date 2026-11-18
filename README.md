# blog-post-skill

一个 [DeepSeek Harness](https://github.com/deepseek-ai) 的 skill：**把一段粗糙的笔记变成一篇可以发布的博客文章。**

给它这样一段话：

> softmax 是用来解决多分类问题，（这里放一张图片画的是多分类问题和二分类问题），如果把样本看作随机事件的话，那它的概率分布就需要满足以下两个条件（这里放两个公式…），为了满足这个条件我们可以用 softmax 函数（这里后面放 softmax 公式）

它会：

- **纠错**——术语太松、逻辑跳步、符号不统一、事实不精确，逐句改；
- **补全**——只补读者一定会卡住的地方（一个数字例子、一个"为什么"、一个坑），不注水；
- **排版**——按站点既有的 HTML 骨架和组件写，不自造样式；
- **落实占位符**——能画的示意图直接画成 SVG，画不了的放带 TODO 的占位框；
- **配公式**——写成 LaTeX（MathJax 渲染），不再用文字描述"概率之和为一"；
- **自检**——重建索引、起本地服务截图，检查公式、图、目录、首屏。

产出是一个 `posts/<slug>.html`，可以直接推上站点。

## 它做出来的东西长什么样

示例输出（真实发布）：<https://zzcym.github.io/posts/softmax.html>
以及这段笔记的改写对照：[`references/example.md`](references/example.md)

## 安装

```bash
git clone https://github.com/zzcym/blog-post-skill.git ~/.dsh/skills/blog-post
```

skill 会被热加载，不用重启。终端里敲 `/`，或者直接对 agent 说"把这段写成博客"即可。

> 也可以放在项目里：`<你的仓库>/.dsh/skills/blog-post/`，只对该仓库生效。

## 目录

```
blog-post-skill/
├── SKILL.md              # 主流程：拆解 → 纠错 → 补全 → 落实占位符 → 自检
├── templates/post.html   # 文章骨架（站点头部、meta、导航、页脚都留好占位）
└── references/
    ├── style.md          # 语气、文章结构、五类纠错清单、排版细节
    ├── components.md     # 可用的 HTML 片段（图/公式/表格/提示块/代码…）
    └── example.md        # 一次完整的改写示范：softmax 笔记 → 成稿
```

`SKILL.md` 是入口，另外三个文件是按需读的参考资料——所以它不会一上来就吃掉几千 token。

## 它是为哪个站点写的

这份 skill 绑定的是 <https://zzcym.github.io> 这个博客的结构：
`tools/build-index.mjs` 建索引、`posts/*.html` 存文章、`assets/css/style.css` 里的一套组件类名、`assets/js/mathjax/` 里的公式支持。

想用在别的站点，改三处就够：

1. `SKILL.md` 顶部 `metadata.blog_root` —— 指向你的仓库；
2. `templates/post.html` —— 换成你站点的文章骨架（head、导航、页脚）；
3. `references/components.md` —— 换掉里面用到的 class 名。

写作规则（`style.md`）和流程（`SKILL.md`）是通用的，不用动。

## 几个刻意的设计

- **默认不 push**：只写文件，用户说"发"才推。写文章和发布应该是两个决定。
- **纠错要能说清**：每条改动都要能回答"原来的说法为什么不对"，而不是按语感润色。
- **补充有上限**：笔记三句话也最多写到 600 字左右；宁可短，不注水。
- **不确定就标注**：拿不准的事写成 `<!-- 待确认：… -->` 并在交付说明里列出来，不编。
- **占位符是意图，不是占位**：作者写下"这里放一张图"时已经想好了那里该有什么，所以要落实，而不是随便塞点东西。

## 环境

- DeepSeek Harness（本 skill 的宿主）
- 自检那一步会用到本机的 Chrome / Edge 无头截图；不装也不影响生成文章，只是少了目视检查

## License

MIT © zzcym
