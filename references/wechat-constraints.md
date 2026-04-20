# 微信公众号排版约束参考

## 核心限制

### 不支持的 CSS 属性
微信公众号编辑器会过滤或忽略以下 CSS：
- `position: fixed / sticky`（会被移除）
- `z-index`（基本无效）
- `transform`（大多数情况被忽略）
- `animation` / `@keyframes`（完全不支持）
- `::before` / `::after` 伪元素（不支持）
- `background-image: url()`（外链背景图会被屏蔽）
- `float`（效果不稳定，避免使用）
- `display: grid`（不支持，改用 `<table>`）
- `display: flex`（❌ 微信粘贴后被完全剥离，禁止写入文章内容层）
- `display: inline-block`（⚠️ 不稳定，上下 padding 粘贴后失效；改用 `<table>` 单元格）
- `overflow: hidden`（❌ 微信编辑器剥离此属性，不要写入文章内容）
- `box-shadow`（⚠️ Android 微信部分版本忽略，不要依赖作为主要视觉元素）
- `text-decoration: line-through`（⚠️ 部分版本剥离，改用 `<s>` 或 `<del>` HTML 标签）
- `width: 100vw` / `height: 100vh`（视口单位无效）
- CSS 变量 `var(--xxx)`（不支持）
- `calc()`（不支持）
- `:hover` 等交互伪类（无效）
- `@media` 媒体查询（无效）
- `@import`（不支持）
- `clip-path`（不支持）
- `box-sizing: border-box`（部分渲染引擎可能忽略）

### 不支持的 HTML/布局
- `<script>` 标签（完全剥离）
- `<link>` 外部样式表（剥离）
- `<iframe>`（剥离）
- `<video>` / `<audio>`（必须用微信内置视频组件，不能直接插入）
- 表单元素（`<input>`, `<button>`, `<form>`）（剥离或失效）
- 外链图片（须先上传至微信素材库）
- 超链接 `<a href="">` 只能指向微信内部链接或白名单域名

### 字体限制
- `@font-face` 自定义字体（不支持）
- Google Fonts / CDN 字体（不支持，被屏蔽）
- 只能使用系统字体：
  - 中文：`"PingFang SC"`, `"Microsoft YaHei"`, `"微软雅黑"`, `"STSong"`, `"SimSun"`
  - 通用：`sans-serif`, `serif`

## 支持的排版方式

### 可靠的 CSS 样式
```css
/* ✅ 这些属性可以放心使用 */
font-size: 16px;           /* 字号，用 px */
font-weight: bold;         /* 加粗 */
color: #333333;            /* 文字颜色 */
background-color: #f5f5f5; /* 背景色（纯色，在 <table>/<td> 上最稳定） */
text-align: center;        /* 对齐 */
line-height: 1.8;          /* 行高 */
letter-spacing: 0.1em;     /* 字间距 */
margin-top: 10px;          /* 外边距（展开写法更可靠，避免缩写） */
margin-bottom: 10px;
padding-top: 15px;         /* 内边距（展开写法） */
padding-right: 20px;
padding-bottom: 15px;
padding-left: 20px;
border-width: 1px;         /* 边框（展开写法） */
border-style: solid;
border-color: #ddd;
border-radius: 8px;        /* 圆角（需配合 table 的 border-collapse: separate） */
border-left-width: 4px;    /* 左边框（常用于引用块） */
border-left-style: solid;
border-left-color: #ff6b6b;
width: 90%;                /* 百分比宽度 */
max-width: 600px;          /* 最大宽度 */
display: block;            /* 块级 */
display: inline;           /* 内联 */
/* ⚠️ display: inline-block — 禁止在文章内容层使用（上下 padding 失效） */
/* ❌ overflow: hidden — 粘贴后被微信剥离，禁止使用 */
text-decoration: underline; /* 下划线 */
word-break: break-all;     /* 换行 */
white-space: pre-wrap;     /* 保留换行 */
opacity: 0.8;              /* 透明度（部分支持） */
/* ⚠️ box-shadow — Android 微信部分版本忽略，不可作为主要视觉元素 */
```

### 图片处理
- 使用 `<img>` 标签，`src` 必须是微信 CDN 地址（`wx2.sinaimg.cn` 等）
- 建议用 `style="width:100%; display:block;"` 控制图片尺寸
- 图片和文字混排需要用 `<table>` 而不是 `float`

### 布局方法与背景色限制
- **背景与边框的渲染陷阱**：微信公众号编辑器在很多时候会过滤或剥离 `<div>` 和 `<section>` 上的 `background-color` 和 `border`。因此，如果需要稳定的背景块或边框模块，**必须使用 `<table>` 结构**。
- **强制清除 Table 边框**：微信公众号会默认给所有的 `<table>`、`<th>`、`<td>` 强加上灰色边框。为了使用 Table 而不出现丑陋的网格，**必须**在 `<table>` 上显式写上 `border="0" cellpadding="0" cellspacing="0"`，并同时在所有的 `<td>` 或 `<th>` 的内联 style 中加上 `border: none;`（除非你需要明显的边框）。
- **⚠️ 全页深色背景不可行**：将整篇文章做成深色背景（如黑底白字）在微信中有两重风险：① `<div>` 背景被剥离导致白字浮在白底不可读；② 微信 App「深色模式」会强制反转颜色破坏排版。深色主题应采用**白底 + 深色强调卡片**方案，用深色 `<td>` 背景实现局部深色区块，而非全页深色底。

## 常用排版模式


### 标题样式
```html
<!-- 主标题 -->
<h1 style="font-size:24px; font-weight:bold; color:#1a1a1a; text-align:center; margin:20px 0; line-height:1.4;">文章标题</h1>

<!-- 副标题 / 章节标题 -->
<h2 style="font-size:18px; font-weight:bold; color:#333; border-left:4px solid #07c160; padding-left:10px; margin:20px 0 12px;">章节标题</h2>
```

### 正文段落
```html
<p style="font-size:16px; color:#3d3d3d; line-height:1.8; margin:10px 0; text-indent:2em;">正文内容，首行缩进两格。</p>
```

### 引用块
```html
<blockquote style="margin:15px 0; padding:12px 16px; background:#f8f8f8; border-left:4px solid #07c160; color:#666; font-size:15px; line-height:1.7;">
  引用内容
</blockquote>
```

### 分割线
```html
<!-- ✅ 用 table 居中（比 div + margin:auto 更可靠） -->
<table style="margin:20px auto;"><tr><td style="width:80%; height:1px; background:#e0e0e0; font-size:0; line-height:0;"> </td></tr></table>

<!-- ❌ 如下写法 margin:auto 在微信可能不生效 -->
<!-- <div style="width:80%; height:1px; background:#e0e0e0; margin:20px auto;"></div> -->
```

### 高亮标注
```html
<!-- ✅ span 内联标签：background-color 和文字颜色可靠，但 padding 上下方不生效 -->
<span style="background-color:#fffbe6; color:#d46b08; font-weight:bold;">重点内容</span>

<!-- ✅ 需要 padding 的内联标签：用 table 单元格 -->
<table><tr><td style="background-color:#fffbe6; color:#d46b08; padding:2px 8px; font-weight:bold;">重点内容</td></tr></table>
```

### 删除线（原价展示）
```html
<!-- ✅ 使用 HTML 标签，比 CSS text-decoration 更可靠 -->
<s style="color:#999;">原价 ¥1980</s>

<!-- ❌ 以下写法部分版本微信会剥离 -->
<!-- <span style="text-decoration:line-through;">原价</span> -->
```

### 卡片容器
```html
<!-- ✅ 用 <table> 实现卡片背景（<div> 的 background/border 在微信中不稳定） -->
<table border="0" cellpadding="0" cellspacing="0" style="width: 100%; border-collapse: separate; border-spacing: 0; border-radius: 8px; border-width: 1px; border-style: solid; border-color: #ebebeb; margin-top: 15px; margin-bottom: 15px;">
  <tr>
    <td style="background-color: #f9f9f9; padding-top: 16px; padding-right: 20px; padding-bottom: 16px; padding-left: 20px; border: none; border-radius: 8px;">
      卡片内容
    </td>
  </tr>
</table>

<!-- ❌ 以下 div 写法的 background 和 border 在微信中可能被剥离 -->
<!-- <div style="background:#f9f9f9; border-radius:8px; padding:16px 20px; margin:15px 0; border:1px solid #ebebeb;">卡片内容</div> -->
```

## 安全颜色参考

### 背景色
- 浅灰：`#f5f5f5` / `#f8f9fa`
- 浅米：`#fffbe6` / `#fef9e7`
- 浅蓝：`#e8f4f8` / `#ebf5fb`
- 浅绿：`#e8f8f0` / `#eafaf1`
- 浅粉：`#fef0f0` / `#fce4ec`

### 文字色
- 主文字：`#1a1a1a` / `#333333`
- 次文字：`#666666` / `#888888`
- 辅助文字：`#999999` / `#bbbbbb`

### 品牌色（微信绿）
- `#07c160`（微信标准绿）
- `#06ad56`（深绿强调）

## 文章宽度
- 微信公众号文章内容区域宽度约 375px（手机）到 677px（PC）
- 始终以手机端为首要设计目标
- 建议最大宽度不超过 600px，使用百分比宽度

## 高风险属性快速参考（实测结论）

| 属性/写法 | 风险等级 | 微信行为 | 安全替代方案 |
|-----------|---------|---------|-------------|
| `display: flex` | ❌ 禁止 | 粘贴后被完全剥离，布局崩溃 | `<table>` 多列布局 |
| `display: inline-block` | ⚠️ 慎用 | 上下 padding 失效，宽高不稳定 | `<table>` 单元格或 `display: block` |
| `overflow: hidden` | ❌ 禁止 | 被直接剥离，不可依赖 | 直接移除，用内容结构控制溢出 |
| `box-shadow` | ⚠️ 慎用 | Android 微信部分版本忽略 | `border` 边框替代装饰感 |
| `text-decoration: line-through` | ⚠️ 慎用 | 部分版本剥离 | `<s>` 或 `<del>` HTML 标签 |
| `margin: auto`（div 居中） | ⚠️ 慎用 | 块级元素居中不可靠 | `text-align: center` 或 `<table style="margin: x auto;">` |
| `max-width` + `margin: 0 auto` | ⚠️ 慎用 | 外层 div 居中在微信内容区无效 | 直接省略，依赖微信内容区宽度 |
| `border-radius` | ⚠️ 陷阱 | 与 `border-collapse: collapse` 冲突 | 带有圆角边框的 table 必须用 `border-collapse: separate; border-spacing: 0;` |
| `<div>` 的 `padding` | ⚠️ 陷阱 | 嵌套的 div 常常被微信剥离 | 必须用到内边距的地方，使用内层 `<table>` 的 `<td>` 加 padding |
| `<span>` 的 `padding` | ⚠️ 慎用 | 上下 padding 通常无效，左右基本可用 | `<table>` 单元格实现内边距 |
| `<table>` 布局 | ✅ 推荐 | 最稳定的多列/居中方案 | — |

## 输出格式说明
- 生成完整 HTML 文件，包含**预览外壳**（工具栏 + JS 切换/复制逻辑）和**文章内容层**（`id="article-content"`）
- 文章内容层内部：所有样式内联（`style="..."` 属性），无 `<script>` / `<style>`
- 复制按鈕仅复制 `#article-content` 内部的 HTML，不含预览外壳
- 内容可直接粘贴到微信公众号编辑器中，格式完全兼容
