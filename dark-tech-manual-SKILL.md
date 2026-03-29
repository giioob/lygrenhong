---
name: dark-tech-manual
description: |
  生成精美的深色主题中文技术手册 HTML 文件。适用于：用户想把教程、操作手册、使用指南、项目介绍等内容制作成一个好看的单页 HTML 文档时。触发词包括但不限于："生成手册"、"制作文档"、"做成 HTML"、"操作指南"、"风格和之前一样"、"深色主题文档"、"技术手册"、"使用说明"。即使用户只说"帮我写一份 XXX 的指南"，也应考虑使用此 skill 生成精美 HTML 文件，而不是输出纯文本。
---

# Dark Tech Manual — 深色技术手册生成器

## 概述

将任意技术内容（教程、操作手册、API 文档、项目介绍等）生成为单文件、开箱即用的精美 HTML 文档。

**设计语言**：深色极简 + 霓虹 Accent + 等宽代码字体 + 卡片式布局。

---

## 设计系统（必须严格遵守）

### CSS 变量（直接复制到 `<style>` 里）

```css
:root {
  --bg:       #0d0f14;   /* 页面背景 */
  --surface:  #13161e;   /* 卡片/TOC 背景 */
  --surface2: #1a1e2a;   /* 次级表面（步骤序号背景） */
  --border:   #252a38;   /* 边框 */
  --accent:   #6ee7b7;   /* 主 Accent：绿色（标题、高亮、TOC 边框） */
  --accent2:  #38bdf8;   /* 副 Accent：蓝色（h3 标题、代码高亮） */
  --accent3:  #f472b6;   /* 第三 Accent：粉色（标签、特殊强调） */
  --warn:     #fbbf24;   /* 警告色：黄色 */
  --text:     #e2e8f0;   /* 主正文颜色 */
  --muted:    #64748b;   /* 辅助文字、注释 */
  --code-bg:  #0a0c10;   /* 代码块背景 */
}
```

### 字体（必须从 Google Fonts 引入）

```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;600;700&family=JetBrains+Mono:wght@400;600&family=Noto+Sans+SC:wght@300;400;500;700&display=swap" rel="stylesheet">
```

| 用途 | 字体 |
|---|---|
| 正文 / 默认 | `Noto Sans SC`, weight 300 |
| 大标题 / 章节标题 | `Noto Serif SC`, weight 700 |
| 代码 / 命令 | `JetBrains Mono` |

---

## 页面结构（按顺序输出）

### 1. Hero 区

```html
<div class="hero">
  <!-- 背景光晕（用 ::before 伪元素，radial-gradient） -->
  <div class="hero-badge">📖 副标题徽章</div>
  <h1>主标题</h1>       <!-- Noto Serif SC，渐变色：白→accent→accent2 -->
  <p>一句话描述</p>
  <div class="hero-tags">
    <span class="tag tag-green">✦ 标签1</span>
    <span class="tag tag-blue">✦ 标签2</span>
    <span class="tag tag-pink">✦ 标签3</span>
    <span class="tag tag-yellow">✦ 标签4</span>
  </div>
</div>
```

**Hero 关键样式：**
- 最小高度 340px，flex 居中
- `::before` 伪元素做两个 radial-gradient 光晕（绿色+蓝色，低透明度）
- h1 用 `background: linear-gradient(135deg, #fff, var(--accent), var(--accent2))` + `-webkit-background-clip: text`

### 2. 目录（TOC）

```html
<div class="toc">
  <div class="toc-title">📋 目录</div>
  <ol>
    <li><a href="#s1">章节标题</a>
      <ul class="sub"><li><a href="#s1-1">子章节</a></li></ul>
    </li>
  </ol>
</div>
```

**TOC 样式：** `border-left: 3px solid var(--accent)`，背景 `var(--surface)`，圆角 12px。

### 3. 章节（Section）

每个章节结构：

```html
<div class="section" id="sN">
  <div class="section-header">
    <div class="section-num">N</div>   <!-- 绿蓝渐变方块 -->
    <h2>章节标题</h2>                  <!-- Noto Serif SC -->
  </div>
  <!-- 内容区：可混用下方所有组件 -->
</div>
```

---

## 核心组件库

### 卡片网格（2列，移动端1列）

```html
<div class="card-grid">
  <div class="card">
    <div class="card-icon">🔧</div>
    <div class="card-title">标题</div>
    <div class="card-desc">描述文字</div>
  </div>
</div>
```
卡片有 hover 边框变 `var(--accent)` 效果。

### 步骤流程

```html
<div class="steps">
  <div class="step">
    <div class="step-num">1</div>
    <div class="step-body">
      <div class="step-title">步骤标题</div>
      <div class="step-desc">步骤描述，可含 <code>inline code</code></div>
    </div>
  </div>
</div>
```
序号圆形，`border: 1.5px solid var(--accent)`，颜色 `var(--accent)`。

### 代码块

```html
<div class="code-block">
  <div class="code-header">
    <div class="code-dots">
      <div class="dot dot-red"></div>
      <div class="dot dot-yellow"></div>
      <div class="dot dot-green"></div>
    </div>
    <div class="code-label">终端 · bash</div>
  </div>
  <pre><span class="cm"># 注释</span>
<span class="kw">export</span> KEY=<span class="st">"value"</span>
command <span class="op">--flag</span></pre>
</div>
```

**代码语法着色 span 类：**
- `.cm`：注释（`var(--muted)`）
- `.kw`：关键字（`var(--accent2)` 蓝色）
- `.st`：字符串（`var(--accent)` 绿色）
- `.op`：操作符/标志（`var(--accent3)` 粉色）

### Callout 提示框

```html
<!-- 三种类型：tip（绿）/ warn（黄）/ info（蓝） -->
<div class="callout callout-tip">
  <div class="callout-icon">💡</div>
  <div class="callout-body"><strong>标题：</strong>描述内容</div>
</div>
```

### 命令速查表

```html
<table class="cmd-table">
  <tr><th>命令</th><th>作用</th></tr>
  <tr>
    <td class="cmd">/command-name</td>
    <td class="cmd-desc">命令描述</td>
  </tr>
</table>
```
`.cmd` 用 `JetBrains Mono`，颜色 `var(--accent)`。

### 横向流程图

```html
<div class="flow">
  <div class="flow-node [active]">
    <div class="flow-node-icon">🧠</div>
    <div class="flow-node-label">节点名</div>
    <div class="flow-node-sub">副标题</div>
  </div>
  <div class="flow-arrow">→</div>
  <!-- 重复... -->
</div>
```
`.active` 节点边框变绿，标签颜色变 `var(--accent)`。

### 技术栈徽章

```html
<div class="stack-row">
  <span class="stack-badge">TypeScript</span>
  <span class="stack-badge">Python</span>
</div>
```

---

## 全局排版规则

- `<p>` 颜色：`#94a3b8`（比 `--text` 略暗）
- `<h3>` 颜色：`var(--accent2)`，前面带 3px 蓝色竖线（用 `::before` 伪元素）
- 行内 `<code>`：背景 `var(--surface2)`，边框 `var(--border)`，颜色 `var(--accent)`，圆角 4px
- 最大内容宽度：860px，水平居中，左右 padding 24px
- 章节间距：`margin-bottom: 56px`

---

## 页脚

```html
<footer>
  <p>文档标题 · 版本信息</p>
  <p><a href="...">链接1</a> · <a href="...">链接2</a></p>
</footer>
```
边框 `border-top: 1px solid var(--border)`，文字 `var(--muted)`，链接色 `var(--accent)`。

---

## 输出规范

1. **单文件**：所有 CSS 内联在 `<style>` 标签，不引用外部 CSS 文件（Google Fonts 除外）
2. **响应式**：`.card-grid` 在 560px 以下变为单列
3. **文件命名**：`[主题]-manual.html`，保存到 `/mnt/user-data/outputs/`
4. **完整性**：必须包含 Hero + TOC + 至少3个章节 + Footer
5. **中文优先**：所有界面文字、注释、标题均使用中文

## 生成流程

1. 理解用户提供的内容主题和章节结构
2. 套用本 skill 的设计系统和组件库
3. 为每个章节选择合适的组件组合（卡片/步骤/代码块/表格等）
4. 生成完整单文件 HTML，保存到输出目录
5. 用 `present_files` 工具提供给用户下载
