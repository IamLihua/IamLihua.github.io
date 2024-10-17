---
title: ai4cet6
tags: ml
categories:
  - note
excerpt: 用ai生成6级单词本
published: true
date: 2024-10-10 12:16:10
---

# 用ai生成6级单词本

## 引用

最近看到了[Ceelog/DictionaryByGPT4: 一本 GPT4 生成的单词书 (github.com)](https://github.com/Ceelog/DictionaryByGPT4)

就想着能不能自己搞个6级词汇本，正好[智谱AI开放平台 (bigmodel.cn)](https://open.bigmodel.cn/finance/resourcepack)这边还有很多白嫖的token没有用

数据来源：[mahavivo/english-wordlists: 常用英语词汇表 (github.com)](https://github.com/mahavivo/english-wordlists/tree/master)

## 代码

```python
from rich import print
from zhipuai import ZhipuAI
import zhipuai
import pandas as pd

client = ZhipuAI(api_key="")  # 请填写您自己的APIKey

propmt="""
# 角色

你是一名中英文双语教育专家，拥有帮助将中文视为母语的用户理解和记忆英语单词的专长，请根据用户提供的英语单词完成下列任务。

## 任务

### 分析词义

- 系统地分析用户提供的英文单词，并用中文以简单易懂的方式解答；

### 列举例句

- 根据所需，为该单词提供至少 3 个不同场景下的使用方法和例句。并且附上中文翻译，以帮助用户更深入地理解单词意义。

### 词根分析

- 用中文分析并展示单词的词根；
- 列出由词根衍生出来的其他单词；

### 词缀分析

- 用中文分析并展示单词的词缀，例如：单词 individual，前缀 in- 表示否定，-divid- 是词根，-u- 是中缀，用于连接和辅助发音，-al 是后缀，表示形容词；
- 列出相同词缀的的其他单词；

### 发展历史和文化背景

- 用中文详细介绍单词的造词来源和发展历史，以及在欧美文化中的内涵

### 单词变形

- 列出单词对应的名词、单复数、动词、不同时态、形容词、副词等的变形以及对应的中文翻译。
- 列出单词对应的固定搭配、组词以及对应的中文翻译。

### 记忆辅助

- 用中文提供一些高效的记忆技巧和窍门，以更好地记住英文单词。

### 小故事

- 用英文撰写一个有画面感的场景故事，包含用户提供的单词。
- 要求使用简单的词汇，100 个单词以内。
- 英文故事后面附带对应的中文翻译。
"""
try:
    df = pd.read_excel('wordlist.xlsx')
    # 提取第一列的内容
    first_column = df.iloc[:, 0]  # iloc[:, 0] 表示选择所有行的第一列
    # 打印第一列的内容
    lines=0
    for item in first_column:
        lines+=1
        if lines<206:
            continue
        print(f"处理第{lines}行:{item}")
        response = client.chat.completions.create(
        model="glm-4",  # 请填写您要调用的模型名称
        messages=[
            {"role": "system", "content": propmt},
            {"role": "user", "content": item}
        ],
        )
        result=response.choices[0].message.content
        with open('result.md', 'a',encoding='utf-8') as f:
            f.write('---------------\n')
            f.write(f'## {item}\n')
            f.write(result+'\n\n')
    
except zhipuai.core._errors.APIRequestFailedError:
    print('敏感词')
except Exception as e:
    print('未知的异常:',e)

```

## 结果

以**abandon**为例：

```markdown
### 分析词义

**abandon** 是一个动词，意为“放弃、抛弃、遗弃”。它通常用于描述某人或某物被完全放弃，不再关心或维护。

### 列举例句

1. **场景：学业**
   - **英文**：He decided to **abandon** his studies to pursue a career in music.
   - **中文**：他决定放弃学业，去追求音乐事业。

2. **场景：感情**
   - **英文**：She felt heartbroken when her boyfriend **abandoned** her.
   - **中文**：当她的男朋友抛弃她时，她感到心碎。

3. **场景：物品**
   - **英文**：The old house was **abandoned** and left to decay.
   - **中文**：那座老房子被遗弃了，任其腐朽。

### 词根分析

- **词根**：**band-**，意为“绑、束缚”。
- **衍生单词**：
  - **band**（乐队、带子）
  - **bond**（债券、纽带）
  - **rebind**（重新绑定）

### 词缀分析

- **前缀**：**a-**，表示“加强语气”或“分离”。
- **词根**：**band-**，意为“绑、束缚”。
- **后缀**：**-on**，用于构成动词。

**相同词缀的单词**：
- **a-**：**abate**（减轻）、**abduct**（绑架）
- **-on**：**andon**（日本的一种生产管理工具）、**pandon**（一种鱼类）

### 发展历史和文化背景

**abandon** 这个词源自古法语 **abandonner**，进一步追溯到拉丁语
**abandonare**，意为“完全放弃”。在中世纪英语中，这个词逐渐演变为现在的形式。在欧美文化中，**abandon**
常用于描述一种彻底的、不留余地的放弃行为，带有强烈的情感色彩，如放弃信仰、抛弃亲人等。

### 单词变形

- **名词**：**abandonment**（放弃、遗弃）
- **形容词**：**abandoned**（被遗弃的）
- **副词**：**abandonedly**（放任地）

**固定搭配和组词**：
- **abandon oneself to**（沉溺于）
  - **英文**：He **abandoned himself to** despair.
  - **中文**：他沉溺于绝望之中。

### 记忆辅助

**记忆技巧**：
1. **联想记忆**：将 **abandon** 与 “a band on”（一个带子在）联想，想象一个带子被解开，象征“放弃”。
2. **谐音记忆**：**abandon** 听起来像 “阿爸ndon”，可以想象一个父亲（阿爸）放弃某事。

### 小故事

**英文**：
In the quiet forest, an old car lay **abandoned**. Its rusted body told stories of forgotten dreams. One day, a curious boy discovered it   
and imagined adventures it once had.

**中文**：
在寂静的森林里，一辆旧车被遗弃在那里。它生锈的车身讲述着被遗忘的梦想。有一天，一个好奇的男孩发现了它，并想象它曾经的冒险故事。
```

## 完整结果

见仓库[IamLihua/ai4cet6: 使用ai生成6级单词本 (github.com)](https://github.com/IamLihua/ai4cet6)

[在线网站](https://iamlihua.github.io/ai4cet6/)

# 免责声明

单词表所有内容均由人工智能生成，不代表本人的观点和立场，请注意分辨其中的错误，若有侵权或者违反法律的地方，请联系我们，我们将在第一时间删除。