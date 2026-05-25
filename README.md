# 交互式百科全书 Skill

把 Claude 变成一本"刷到天亮"的百科全书——每次提问返回一张精美的 HTML 卡片，底部 5 个「继续探索」按钮，点击就跳到下一个词条，无限漫游。

## 它做什么

普通 Claude 回答是段落文字。装上这个 skill，你问"什么是寒武纪生命大爆发"，会得到一张结构化卡片：

- **分类标签**（按学科领域配色）+ **词条标题** + **英文副标题**
- **3 格数据统计栏**：选最有冲击力的数字（时间跨度、数量极值、百分比…）
- **3 段正文**，每段高亮 2-3 个核心关键词
- **1-3 个可视化模块**（按词条类型选）：时间线、理论对比、类型展示、数据表、实验案例、引用框、结构图…
- 底部 **5 个「继续探索」按钮**

5 个按钮覆盖知识的 5 种关系——

| 关系类型 | 含义 | 例子 |
|---|---|---|
| 上钻 Zoom out | 当前词条所属的更大框架 | 三叶虫 → 古生代 |
| 下探 Zoom in | 当前词条的具体子概念 | 寒武纪 → 奇虾 |
| 横联 Lateral | 同级别的相关概念 | 寒武纪生命大爆发 → 雪球地球 |
| 因果 Causal | 引发当前词条的原因或其导致的结果 | — |
| 意外联系 Surprise | 想不到但点击后会"原来如此"的词条 | — |

点任意一个按钮，进入下一个词条。形成无限漫游。

## 安装

最简单的方式（直接拷文件到 Claude Code 的 skills 目录）：

```bash
mkdir -p ~/.claude/skills/interactive-encyclopedia
curl -o ~/.claude/skills/interactive-encyclopedia/SKILL.md \
  https://raw.githubusercontent.com/huangyixinrose/interactive-encyclopedia/main/SKILL.md
```

或者 clone 整个仓库：

```bash
git clone https://github.com/huangyixinrose/interactive-encyclopedia.git \
  ~/.claude/skills/interactive-encyclopedia
```

装好后重启 Claude Code（或开新 session），skill 会自动被识别。

## 怎么用

跟 Claude 说下面任意一句话即可触发：

- "你是一个百科全书，介绍一下 X"
- "请介绍 X"
- "我想了解 X"
- "像百科全书一样讲讲 X"
- "XXX 是什么"（在百科会话上下文里）

然后点卡片底部的按钮继续漫游。每张卡片下面还会附 1-2 句话的彩蛋冷知识。

## 设计原则

**知识是网状的，不是线性的。**

维基百科之所以让人刷到天亮，是因为每个词条都通向 5-10 个相关词条。本 skill 把这个机制压缩进每一张 Claude 回复里——让 LLM 不止是"回答问题"，而是"打开一扇知识的门"。

5 种探索关系（上钻 / 下探 / 横联 / 因果 / 意外）不是随便挑的——它们对应人在好奇时的 5 种思维方向。每张卡片都同时给出这 5 个入口，模拟真实的"知识漫游"体验。

## 适合谁

- 喜欢"刷维基百科到天亮"的人
- 希望 Claude 回答有视觉层次的人
- 学生、研究者、终身学习者
- 想给孩子做"看不腻的百科"的家长

## License

[MIT](./LICENSE)

## 作者

Rose · [@huangyixinrose](https://github.com/huangyixinrose)

欢迎提 issue、PR、或者直接 fork 改造成你想要的样子。
