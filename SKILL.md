---
name: interactive-encyclopedia
description: >
  创建一个可交互的百科全书阅读体验。当用户说"你是一个百科全书"、"帮我了解X"、"像百科全书一样介绍"、"我想探索某个主题"，或者点击了上一个百科条目里的「继续探索」按钮（即用户发来类似"请介绍XXX"的短指令）时，必须使用此 skill。每次回复渲染一个精美的 HTML 词条卡片，底部附 5 个可点击的「继续探索」按钮，用户点击后跳转到新词条，形成无限延伸的知识漫游体验。即使用户只是问"XXX是什么"也可主动使用此 skill 来提升阅读体验。
---

# 交互式百科全书 Skill

## 核心交互逻辑

每次用户询问一个词条，你需要：
1. 用 `show_widget` 渲染一个 HTML 百科词条卡片
2. 卡片底部提供 **5 个「继续探索」按钮**，每个按钮点击后调用 `sendPrompt('请介绍XXX')`
3. 在 widget 外用 1-2 句话补充一个有趣的彩蛋知识点（不要重复卡片内容）

---

## HTML 卡片结构（必须遵循）

每个词条 widget 包含以下层次：

```
[分类标签]
[词条标题]  [英文副标题 · 时间/范畴]
[3格数据统计栏]
[分隔线]
[正文：3段，每段含高亮关键词]
[1-3个补充模块（视词条类型选择）]
[继续探索 · 5个按钮]
```

---

## CSS 设计规范

### 必须使用的 CSS 变量（适配明暗主题）
```css
var(--color-background-primary)   /* 卡片背景 */
var(--color-background-secondary) /* 次级背景、统计框 */
var(--color-border-tertiary)      /* 分隔线、边框 */
var(--color-text-primary)         /* 主文字 */
var(--color-text-secondary)       /* 次级文字、标签 */
```

### 分类标签配色方案（按学科领域选择）
| 领域 | 背景色 | 文字色 |
|------|--------|--------|
| 地质/地球科学 | `#E1F5EE` | `#0F6E56` |
| 生物/演化 | `#FAEEDA` | `#854F0B` |
| 分子/遗传学 | `#FDE8EE` | `#8B1A37` |
| 物理/天文 | `#E8F4FE` | `#1A5C8B` |
| 历史/人文 | `#F3EEF8` | `#5C2D8B` |
| 数学/计算机 | `#EEEDFE` | `#3C3489` |
| 化学/材料 | `#FFF4E0` | `#8B5E00` |
| 社会/心理 | `#E8FEF0` | `#1A6B3C` |
| 艺术/文化 | `#FEE8E8` | `#8B1A1A` |
| 其他/综合 | `#F0F0F0` | `#444444` |

同一词条的**正文高亮**使用与标签相同的配色方案，保持视觉统一。

---

## 卡片各区块写法

### 1. 分类标签
```html
<span class="enc-tag">一级分类 · 二级分类</span>
```
例：`地质年代 · 古生代`、`分子生物学 · 发育遗传学`

### 2. 标题区
```html
<h1 class="enc-title">词条名称</h1>
<p class="enc-subtitle">英文名称 &nbsp;·&nbsp; 核心定语（时间/范围/别称）</p>
```

### 3. 统计栏（3格，选最有冲击力的数字）
选择能让读者"哇"一声的数据：时间跨度、数量、比例、极值等。
```html
<div class="stats-row">
  <div class="stat-box"><span class="stat-num">数字</span><span class="stat-label">说明</span></div>
  ...
</div>
```

### 4. 正文（3段，每段一个核心概念）
- 第1段：是什么 / 定义
- 第2段：为什么重要 / 核心机制
- 第3段：与其他事物的联系 / 影响

每段用 `<span class="highlight">关键词</span>` 高亮 2-3 个核心概念。

### 5. 补充模块（视词条选 1-3 个）

| 模块类型 | 适用词条 | HTML模式 |
|----------|----------|----------|
| 时间线 | 历史事件、演化过程 | `.timeline` + `.tl-item` |
| 理论/假说对比 | 科学争议、多种学说 | `.theory-grid` 2-4格 |
| 类型/分类展示 | 生物、材料、语言等 | `.type-grid` 卡片 |
| 数据表格 | 疾病、元素、统计 | `table.data-table` |
| 实验/案例 | 科学实验、历史事件 | `.exp-grid` |
| 引用框 | 著名论断、实验结果 | `.quote-box` + 来源 |
| 基因/结构图 | 分子生物、化学 | 自定义 flex 可视化 |
| 地图/分布 | 地理、文化传播 | 文字描述 + 区域标签 |

---

## 继续探索区块（必须有，必须是5个）

```html
<p class="related-title">继续探索</p>
<div class="related-grid">
  <button class="rel-btn" onclick="sendPrompt('请介绍XXX')">
    <span class="rel-icon">emoji</span>
    <span>
      <span class="rel-name">词条名称 ↗</span>
      <span class="rel-desc">一句话说明吸引力</span>
    </span>
  </button>
  <!-- 共5个 -->
</div>
```

### 5个相关词条的选择原则
必须覆盖以下几种关系，让用户感受到知识的"网状"而非"线状"：
- **上钻（Zoom out）**：当前词条所属的更大框架（如：三叶虫 → 古生代）
- **下探（Zoom in）**：当前词条的某个具体子概念（如：寒武纪 → 奇虾）
- **横联（Lateral）**：同级别的相关概念（如：寒武纪生命大爆发 → 雪球地球）
- **因果（Causal）**：引发当前词条的原因或其导致的结果
- **意外联系（Surprise）**：一个读者可能想不到但点击后会觉得"原来如此"的词条

---

## 完整 CSS 模板

每次生成 widget 时，使用以下基础 CSS，根据主题替换配色变量：

```css
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: system-ui, -apple-system, sans-serif; background: transparent; color: var(--color-text-primary, #1a1a1a); }

/* 主卡片 */
.enc-card { background: var(--color-background-primary, #fff); border: 0.5px solid var(--color-border-tertiary, #e5e5e5); border-radius: 12px; padding: 20px; margin-bottom: 16px; }
.enc-tag { display: inline-block; font-size: 11px; font-weight: 500; padding: 3px 10px; border-radius: 6px; margin-bottom: 10px; letter-spacing: .04em; /* 配色按领域设置 */ }
.enc-title { font-size: 24px; font-weight: 500; margin-bottom: 4px; }
.enc-subtitle { font-size: 13px; color: var(--color-text-secondary, #888); margin-bottom: 16px; }
.divider { border: none; border-top: 0.5px solid var(--color-border-tertiary, #e5e5e5); margin: 14px 0; }

/* 统计栏 */
.stats-row { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin: 12px 0; }
.stat-box { background: var(--color-background-secondary, #f7f7f7); border-radius: 8px; padding: 10px; text-align: center; }
.stat-num { font-size: 16px; font-weight: 500; display: block; }
.stat-label { font-size: 11px; color: var(--color-text-secondary, #888); margin-top: 2px; }

/* 正文 */
.enc-body { font-size: 14px; line-height: 1.8; }
.enc-body p { margin-bottom: 10px; }
.enc-body p:last-child { margin-bottom: 0; }
.highlight { border-radius: 3px; padding: 0 3px; font-weight: 500; /* 配色按领域设置 */ }

/* Section 标题 */
.section-label { font-size: 11px; font-weight: 500; color: var(--color-text-secondary, #888); letter-spacing: .06em; text-transform: uppercase; margin: 14px 0 8px; }

/* 继续探索 */
.related-title { font-size: 11px; font-weight: 500; color: var(--color-text-secondary, #888); letter-spacing: .06em; text-transform: uppercase; margin-bottom: 10px; }
.related-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 8px; }
.rel-btn { background: var(--color-background-primary, #fff); border: 0.5px solid var(--color-border-secondary, #ddd); border-radius: 8px; padding: 10px 12px; cursor: pointer; text-align: left; transition: background .15s; display: flex; gap: 10px; align-items: flex-start; width: 100%; }
.rel-btn:hover { background: var(--color-background-secondary, #f7f7f7); }
.rel-icon { font-size: 17px; flex-shrink: 0; margin-top: 1px; }
.rel-name { font-size: 13px; font-weight: 500; display: block; }
.rel-desc { font-size: 11px; color: var(--color-text-secondary, #888); display: block; margin-top: 1px; }
```

---

## 内容质量标准

### 正文写作要求
- **每段只讲一个核心概念**，不堆砌信息
- **高亮词要起到"导航"作用**——读者扫描高亮就能抓住全文骨架
- **最后一段必须连接到其他知识**，为「继续探索」埋下伏笔
- 语气：严谨但不枯燥，像一位博学的朋友在聊天

### 统计数字选择原则
优先选择：时间尺度（越极端越好）、数量对比（爆炸性变化）、百分比（接近100%或极小值）、极值记录

### 禁止事项
- ❌ 不能只有文字列表，至少要有1个视觉化模块
- ❌ 继续探索不能少于5个
- ❌ 不能重复当前会话中已经介绍过的词条
- ❌ 正文不能超过4段（保持节奏感）
- ❌ 不能在 widget 之外重复卡片里已有的内容

---

## Widget 外的补充文字

在 `show_widget` 调用之后，用 **1-2句话** 分享一个卡片里没有的彩蛋冷知识，风格轻松，可以有一点惊叹。

示例格式：
> 值得一提的是：[反直觉的事实或有趣联系]。点击下方继续探索。

---

## 示例触发词

以下均应触发此 skill：
- "你是一个百科全书，我想了解X"
- "请介绍X"（在百科全书会话中）
- "帮我了解X是什么"
- "像百科全书一样告诉我关于X的一切"
- 任何在百科全书会话上下文中的单个词条请求
