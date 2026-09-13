---
name: english-word-explainer
description: 当用户的回复本体只有一个英语单词或一个英语词组时触发，即使用户没有明确要求「解释」——只要回复的核心是单个英语单词/词组（可带末尾标点，可夹杂少量提问性中文如「XX什么意思」），就应当使用本技能给出结构化释义。也适用于多行输入：每行一个英语单词/词组时，按多次输入逐一处理；允许合并互为派生、相近、容易产生联想或易混淆的单词/词组。输出包含国际音标发音、按词性分列的中文释义、近义词、反义词、例句及中文翻译、常用固定搭配（含例句与翻译）、词根拆分与联想记忆，并严格遵守「无 emoji、直角引号「」、仅用无序列表」的排版规范。若输入是完整的英语句子、问句或段落，则不触发本技能。
---

# english-word-explainer

用户回复中出现单个英语单词或英语词组时，把它当成一次「查词典 + 背单词」请求来处理：给出完整、精确、可直接背诵的结构化释义。本技能的排版规范非常严格，动手输出前先通读「排版规则」一节，并对照文末「完整示例」。

## 触发条件

- 用户回复本体只有**一个英语单词**，或**一个英语词组**：触发。
  - 单词允许带末尾标点（如 `perplex.`），按去掉标点后的词条处理。
  - 回复中除目标词条外若只有少量提问性中文（如「XX是什么意思」「解释一下 XX」），同样触发，围绕该词条输出完整结构。
- 用户回复为**多行**、每行一个英语单词/词组：按多次输入处理——每个词条独立输出一套完整结构，顺序排列；若符合「派生词合并规则」，则先合并再输出。
- 用户回复是完整的英语句子、问句、段落、文章：**不触发**，按普通对话处理。
- 一行内出现多个互不关联的单词（非约定俗成的词组）：不视为单条输入，按普通对话处理。
- 大小写不敏感；专有名词、缩写（如 `NASA`、`per se`）同样按单词/词组处理，释义需体现其专有含义。
- 触发后，首先输出该输入内容，空一格，然后贴上 `#card` 标签，如 `- foo #card`。该标题行之后的所有输出内容相对标题行缩进一级。

## 词条标记格式

输出时，每个词条的标题行必须加上 `#card` 标记：

```
- <词条内容> #card
```

示例：

```
- perplex #card
```

多行输入时，每个词条独立一行：

```
- perplex #card
- enforce #card
```

标签与词条之间空一格。标题行之后的所有输出内容均相对标题行缩进一级。

## 派生词合并规则

当用户输入多个互为派生、相近、容易产生联想或易混淆的单词/词组时，合并输出并进行词义比较。

### 合并条件

以下情况应合并到同一个输出块中：

1. **同词族派生**：如 `enforce`、`enforced`、`enforcement`；`lack`、`lacking`。
2. **相同前缀**：如 `enforce`、`endorse`、`enable`（都是 `en-` 前缀）。
3. **相同后缀**：如 `normalize`、`energize`、`customize`（都是 `-ize` 后缀）。
4. **字形相近、容易产生联想或易混淆**：如 `strike`、`strive`、`stroke`。

### 合并输出格式

#### 派生关系（同一词族）

```
- enforce enforced enforcement #card
  - 发音：英 /ɪnˈfɔːs/；美 /ɪnˈfɔːrs/。
  - 词义比较
    - enforce (v.)：强制执行；实施
    - enforced (adj.)：强制的；被实施的
    - enforcement (n.)：执行；实施；强制
  - 近义词：compel（强迫）；mandate（命令）；implement（实施）
  - 反义词：neglect（忽视）；ignore（忽略）
  - 例句
    - The government enforced the new regulations strictly.
      - 政府严格执行了新法规。
    - The enforced silence was unbearable.
      - 被迫的沉默令人难以忍受。
    - The enforcement of the law requires public cooperation.
      - 法律的执行需要公众合作。
  - 词根拆分
    - en-（使）+ force（力量），源自拉丁语 infortiare。
  - 联想记忆
    - en（使）+ force（力量）→ 施加力量使其执行 → 强制执行。
```

#### 易混淆关系（不同词族）

```
- strike vs strive vs stroke #card
  - 发音
    - strike 英 /straɪk/；美 /straɪk/
    - strive 英 /straɪv/；美 /straɪv/
    - stroke 英 /strəʊk/；美 /stroʊk/
  - 词义比较
    - strike (v.)：打击；罢工；突然想到
    - strive (v.)：努力；奋斗；力争
    - stroke (n.)：一笔；击打；中风
  - 近义词
    - strike：hit（打）；protest（抗议）
    - strive：struggle（挣扎）；endeavor（努力）
    - stroke：hit（击）；brush（刷）
  - 反义词
    - strike：appease（安抚）；cooperate（合作）
    - strive：relax（放松）；give up（放弃）
    - stroke：hesitate（犹豫）
  - 例句
    - strike: The workers went on strike for higher wages.
      - 工人们罢工要求提高工资。
    - strive: She strives to achieve her goals despite difficulties.
      - 她努力克服困难实现目标。
    - stroke: The painter added one final stroke to the canvas.
      - 画家给画布添加了最后一笔。
```

### 词组易混淆处理

词组之间若容易混淆，使用软换行（`
`）间隔：

```
- try to do something
  try doing something #card
```

## 输出结构（按顺序）

对每个输入词条，依次输出以下内容；每一类内容至少占一个独立的无序列表项。该无序列表项相较于带 `#card` 标签的输入内容原文，应该缩进一级：

1. 发音：用国际音标（IPA）标注，英式与美式都要给出（若适用）：`发音：英 /.../；美 /.../。`
2. 中文释义：必须包含词性标注。不同词性、或同一词性下含义差异较大的释义，各另起一行、各占一个列表项。词性标注规则见「词性标注表」；词组用中文「词组」标注，不在表内的词性用中文描述。
3. 近义词：列出若干近义词，每个后用括弧标注其对应释义。
4. 反义词：列出若干反义词，每个后用括弧标注其对应释义。
5. 例句：给出一个或多个英文例句，每个例句下缩进一层给出其中文释义。
6. 常用固定搭配（**仅单词**）：若有，列出常用搭配；每个搭配后用括弧标注其中文含义，并为每个搭配各造一个例句、给出例句的中文释义。词组输入跳过本节。
7. 词根拆分（**仅单词**）：拆出前缀/词根/后缀并给出各自含义，注明词源（如拉丁语、希腊语）。词组输入跳过本节。
8. 联想记忆（**仅单词**）：基于词根拆分或发音、字形给出联想记忆方式。词组输入跳过本节。

## 词性标注表

| 标注 | 含义 |
| --- | --- |
| a. | 形容词 |
| ad. | 副词 |
| n. | 名词 |
| v. | 动词 |
| int. | 感叹词 |
| num. | 数字词 |
| art. | 冠词 |
| prep. | 介词 |
| conj. | 连词 |
| pron. | 代词 |
| idiom. | 习语 |
| phr. | 词组 |

- 上述列表之外出现的词性（如「限定词」），用中文描述，不用缩写。
- 词组统一用 `phr.` 标注，不在上述表内的短语用中文描述。

## 排版规则（硬性要求）

- 所有引号一律使用直角引号「」，不用弯引号或直引号。
- 输出中**不得出现任何 emoji 符号**，使用平凡（plain）文本。
- 除了无序列表，不使用任何其他 Markdown 语法：不加粗、不斜体、不删除线、不写标题、不用代码块、不用表格。
- 每一类输出内容至少处于不同的无序列表项中；需要多行的内容用缩进嵌套的无序列表呈现层次。
- 一行内能展示完的内容：在描述性词语（如「近义词」「反义词」）后加冒号，与内容同行呈现。
- 一行内展示不完的内容：拆成多行，**不加冒号**，用缩进嵌套的无序列表呈现（参照「例句」「常用固定搭配」的示例写法）。
- 不同的输出内容至少应处于不同的无序列表项中。
- 标题行 `- <词条内容> #card` 之后的所有输出内容相对标题行缩进一级。

## 例句格式

「例句」一节统一使用两层缩进嵌套：

```
- 例句
  - <英文例句>
    - <中文释义>
```

「常用固定搭配」内的例句使用 `Example:` 前缀，共三层缩进：

```
- <搭配>（<中文含义>）
  - Example: <英文例句>
    - <中文释义>
```

## 完整示例

### 示例一：单单词输入

输入：

```
perplex
```

输出：

```
- perplex #card
  - 发音：英 /pəˈpleks/；美 /pərˈpleks/。
  - v. 使困惑；使迷惑；使为难。
  - v. 使复杂化；使难解。
  - 近义词：confuse（使困惑）；puzzle（使迷惑）；baffle（使困惑）；mystify（使困惑）；bewilder（使迷惑）；confound（使困惑）。
  - 反义词：enlighten（启发）；clarify（澄清）；elucidate（阐明）；explain（解释）；simplify（简化）。
  - 例句
    - The complex rules perplexed the new employees.
      - 这些复杂的规则让新员工感到困惑。
  - 常用固定搭配
    - be perplexed by（被……困惑）
      - Example: He was perplexed by the sudden change in plans.
        - 他突然改变计划让他感到困惑。
    - perplex someone with something（用某事使某人困惑）
      - Example: She perplexed the audience with her ambiguous answer.
        - 她用模棱两可的回答使观众感到困惑。
    - perplexing question（令人困惑的问题）
      - Example: It is a perplexing question that no one can answer.
        - 这是一个无人能回答的令人困惑的问题。
  - 词根拆分
    - per-（完全，贯穿）+ plex（编织，折叠），源自拉丁语 perplexus。
  - 联想记忆
    - per（完全）+ plex（编织）→ 所有线索完全编织缠绕在一起，理不清，因此使人困惑。
```

### 示例二：派生词合并输入

输入：

```
enforce
enforced
enforcement
```

输出：

```
- enforce enforced enforcement #card
  - 发音：英 /ɪnˈfɔːs/；美 /ɪnˈfɔːrs/。
  - 词义比较
    - enforce (v.)：强制执行；实施
    - enforced (adj.)：强制的；被实施的
    - enforcement (n.)：执行；实施；强制
  - 近义词：compel（强迫）；mandate（命令）；implement（实施）
  - 反义词：neglect（忽视）；ignore（忽略）
  - 例句
    - The government enforced the new regulations strictly.
      - 政府严格执行了新法规。
    - The enforced silence was unbearable.
      - 被迫的沉默令人难以忍受。
    - The enforcement of the law requires public cooperation.
      - 法律的执行需要公众合作。
  - 词根拆分
    - en-（使）+ force（力量），源自拉丁语 infortiare。
  - 联想记忆
    - en（使）+ force（力量）→ 施加力量使其执行 → 强制执行。
```

### 示例三：易混淆词对比输入

输入：

```
strike
strive
stroke
```

输出：

```
- strike vs strive vs stroke #card
  - 发音
    - strike 英 /straɪk/；美 /straɪk/
    - strive 英 /straɪv/；美 /straɪv/
    - stroke 英 /strəʊk/；美 /stroʊk/
  - 词义比较
    - strike (v.)：打击；罢工；突然想到
    - strive (v.)：努力；奋斗；力争
    - stroke (n.)：一笔；击打；中风
  - 近义词
    - strike：hit（打）；protest（抗议）
    - strive：struggle（挣扎）；endeavor（努力）
    - stroke：hit（击）；brush（刷）
  - 反义词
    - strike：appease（安抚）；cooperate（合作）
    - strive：relax（放松）；give up（放弃）
    - stroke：hesitate（犹豫）
  - 例句
    - strike: The workers went on strike for higher wages.
      - 工人们罢工要求提高工资。
    - strive: She strives to achieve her goals despite difficulties.
      - 她努力克服困难实现目标。
    - stroke: The painter added one final stroke to the canvas.
      - 画家给画布添加了最后一笔。
```

### 示例四：易混淆词组对比输入

输入：

```
try to do something
try doing something
```

输出：

```
- try to do something
  try doing something #card
  - 发音：英 /traɪ tuː duː/；美 /traɪ tu du/。
  - 词义比较
    - try to do something (phr.)：努力做某事（强调意图和尝试，不一定成功）
    - try doing something (phr.)：尝试做某事（强调试验某种方法，看效果如何）
  - 近义词
    - try to do something：attempt to do（试图做）；endeavor to do（尽力做）
    - try doing something：test out（测试）；experiment with（试验）
  - 例句
    - try to do something: He tried to open the window but it was stuck.
      - 他努力想打开窗户，但窗户卡住了。
    - try doing something: You should try adding some sugar to balance the flavor.
      - 你应该试试加点糖来平衡味道。
```

## 多行输入处理

当用户输入多行时，每行独立处理，按顺序输出；若符合「派生词合并规则」，则先合并再输出：

输入：

```
perplex
enforce
```

输出：先输出 perplex 的完整结构，再输出 enforce 的完整结构，中间空一行分隔。

输入：

```
enforce
enforced
enforcement
```

输出：合并为一个 `- enforce enforced enforcement #card` 输出块。

## 注意事项

- 派生词合并时，重点展示词形变化和词义差异，帮助用户理解词族的内在联系。
- 易混淆词对比时，重点突出语义重心和使用场景的区别。
- 词组对比时，重点说明语法结构和语用差异。
- 合并重点关注字形相近、容易产生联想和容易混淆的单词。
- 所有内容严格遵守排版规范，不得出现 emoji、粗体、斜体、删除线、标题、代码块、表格等。
- 每个输出部分至少是一个独立的无序列表项。
- 能用一行展示的内容，在描述词后加冒号同行呈现；不能一行的拆成多行，用嵌套列表。
