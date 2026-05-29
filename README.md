# 可以不断探索的交互式百科全书

把 Claude 变成一本漂亮的、活动的，可无限漫游的百科全书。

## 设计原则

**知识是网状的，不是线性的。**

好奇心生生不息，知识环环相扣，我想一直探索下去；机制和原理不能只有干巴的文字描述，我想看见它到底是怎么运作的。

5 种探索关系（上钻 / 下探 / 横联 / 因果 / 意外）——它们对应人在好奇时的 5 种思维方向。每张卡片都同时给出这 5 个入口，模拟真实的"知识漫游"体验。

机制和原理，通过动画的方式让它模拟运作起来。

## 适合谁

- 探索不停，总有新问题的好奇宝宝
- 希望知识成网、机制运作起来的终身学习者
- 希望 Claude 回答有视觉层次的人
- 想给孩子做"看不腻的百科"的家长

## 它做什么

普通 Claude 回答是段落文字。装上这个 skill,你问"什么是寒武纪生命大爆发",会得到一张结构化卡片:

- **分类标签**(按学科领域配色) + **词条标题** + **英文副标题**
- **3 格数据统计栏**:选最有冲击力的数字(时间跨度、数量极值、百分比…)
- **3 段正文**,每段高亮 2-3 个核心关键词
- **1-3 个可视化模块**(按词条类型选):时间线、理论对比、类型展示、数据表、实验案例、引用框、结构图…
- 底部 **5 个「继续探索」按钮**

5 个按钮覆盖知识的 5 种关系——

| 关系类型 | 含义 | 例子 |
|---|---|---|
| 上钻 Zoom out | 当前词条所属的更大框架 | 三叶虫 → 古生代 |
| 下探 Zoom in | 当前词条的具体子概念 | 寒武纪 → 奇虾 |
| 横联 Lateral | 同级别的相关概念 | 寒武纪生命大爆发 → 雪球地球 |
| 因果 Causal | 引发当前词条的原因或其导致的结果 | — |
| 意外联系 Surprise | 想不到但点击后会"原来如此"的词条 | — |

点任意一个按钮,进入下一个词条。形成无限漫游。

<p align="center">
  <a href="https://github.com/huangyixinrose/skill-interactive-encyclopedia/blob/main/assets/demo.mov">▶ 观看 demo 演示视频</a>
</p>

## 安装

最简单的方式（直接拷文件到 Claude Code 的 skills 目录）：

```bash
mkdir -p ~/.claude/skills/interactive-encyclopedia
curl -o ~/.claude/skills/interactive-encyclopedia/SKILL.md \
  https://raw.githubusercontent.com/huangyixinrose/skill-interactive-encyclopedia/main/SKILL.md
```

或者 clone 整个仓库：

```bash
git clone https://github.com/huangyixinrose/skill-interactive-encyclopedia.git \
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

## License

[MIT](./LICENSE)

## 作者

Rose · [@huangyixinrose](https://github.com/huangyixinrose)

欢迎提 issue、PR、或者直接 fork 改造成你想要的样子。
