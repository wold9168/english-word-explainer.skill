# english-word-explainer

将「用户回复单个英语单词/词组 → 输出结构化中文释义」的流程固化为独立 skill 的仓库。

## 功能

输入一个英语单词或英语词组，输出：

- 国际音标（IPA）发音（英式 + 美式）
- 按词性分列的中文释义
- 近义词 / 反义词（带对应释义括注）
- 英文例句及中文翻译
- 常用固定搭配（含例句与翻译，仅单词）
- 词根拆分与联想记忆（仅单词）

排版遵守：无 emoji、直角引号「」、仅使用无序列表（可缩进嵌套）。

## 目录

- `SKILL.md` — 技能本体（含触发条件、输出结构、词性标注表、排版规则、完整示例）

## 安装

方式一（推荐，与 `~/.dsh/skills/` 下其他 skill 仓库一致：仓库放 `~/.dsh/skillrepo/`，skills 目录放软链）：

```bash
cp -r english-word-explainer ~/.dsh/skillrepo/
ln -s ../skillrepo/english-word-explainer ~/.dsh/skills/english-word-explainer
```

方式二（直接把目录放进 skills）：

```bash
cp -r english-word-explainer ~/.dsh/skills/
```

## 使用

安装后无需任何命令。只要在对话中回复一个英语单词或词组（或多行、每行一个），即自动触发完整释义输出。
