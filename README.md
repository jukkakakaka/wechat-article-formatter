# WeChat Article Formatter (微信公众号精美排版技能)

将普通文本或 Markdown 草稿，转化为符合微信公众号 CSS 兼容性约束、可直接一键复制并粘贴至微信官方编辑器的排版精美 HTML。

## 🌟 核心功能

- **多风格支持**：为不同场景提供「商务精英」、「温情故事」、「科技极客」、「文艺清新」、「知识干货」、「营销推广」等多种预设排版风格。
- **内置排版预览模式**：生成的 HTML 附带双端视角切换功能，可快速切换“手机预览（390px）”和“电脑预览（677px）”。
- **一键无缝粘贴**：包含“复制到公众号”剪贴板辅助逻辑，解决传统复制导致的样式丢失、本地图片死图等微信特有兼容性问题。
- **微信严格兼容**：深度提炼整理微信公众号编辑器（如 `table` 布局适配、禁止外部字体与动画等）的各种隐藏坑点。彻底解放日常排版工作！

## 📂 结构说明

- `SKILL.md`：核心 Prompt 指令和规则定义。在这里查看/调整各种风格体系及技术强制约束。
- `references/`：关于微信公众号底层 HTML 支持与 CSS 的兼容避坑指南（尤其是各类排版玄学现象的破局方案）。
- `evals/`：自带技能评测配置文件，快速测试输出排版的稳定性。

## 🚀 极速上手

1. 在具有大模型技能/代码生成能力的环境下安装本技能。
2. 告诉模型你要排版的文章内容，并提一句："**帮我根据商务风格排版**" 或者 "**转成微信公众号排版**"。
3. 你将得到一个 `.html` 文件，右键用浏览器打开，即可所见即所得地预览，一键复制后去微信发文！

## 🧪 Evals 案例展示

通过 `evals.json` 测试框架生成的三个典型风格排版案例。以完整的浏览器预览外壳呈现，可直接点击下方链接进行真实的网页渲染效果预览（支持查看手机与网页双端视角）：

- 🔗 **[案例一：科技感风格（深色背景、卡片分离、高亮结论）](https://htmlpreview.github.io/?https://github.com/xujundear/wechat-article-formatter/blob/main/examples/eval-1-tech.html)**
- 🔗 **[案例二：温情散文风格（暖色调、大行距、诗意引用）](https://htmlpreview.github.io/?https://github.com/xujundear/wechat-article-formatter/blob/main/examples/eval-2-warmth.html)**
- 🔗 **[案例三：营销推广风格（大红色高对比度、编号卡片、价格突出）](https://htmlpreview.github.io/?https://github.com/xujundear/wechat-article-formatter/blob/main/examples/eval-3-promo.html)**

> 💡 *提示：点击链接后，由于采用了 Github HTML 渲染服务，加载可能需要等待 1~3 秒钟。加载完成后可点击顶部工具栏进行体验。*
