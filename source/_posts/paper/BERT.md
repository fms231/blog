---
title: BERT
date: 2023-08-01 00:00:00
---

# BERT：Bidirectional Encoder Representations from Transformers
BERT是将词语转换为向量的模型，诞生源于Transformer。由于Transformer的Encoder能将语言的意义很好的抽离出来，那么将Encoder部分独立，也许能很好的对语言做出表示。因此，BERT的核心部分就是Encoder，除此之外，BERT还结合了ELMo的思想，ELMo的核心部分是由双向LSTM组成，BERT结合两者的优点，核心部分由双向Encoder组成。
Bert诞生于2018年，此后在各类的NLP比赛中疯狂屠榜，是集大成者，公认的里程碑。
## 单向编码和双向编码
单向编码：只能看到当前时刻之前的词，不能看到当前时刻之后的词。
双向编码：可以看到当前时刻之前的词，也可以看到当前时刻之后的词。
## Bert的参数
Bert采用双向编码，Bert的参数量很大，因此Bert的训练时间很长，训练花费大，我们大多数情况下都是使用预训练好的Bert模型，然后在此基础上进行微调。
bert参数：
L = 12，H = 768，A = 12，参数量为1.1亿。
L = 24，H = 1024，A = 16，参数量为3.4亿。
L为层数，H为隐藏层的维度，A为多头注意力的头数。
## Bert的训练
第一阶段：使用易获取的大规模无标记的数据进行预训练，目标是学习通用的语言表示。
第二阶段：使用带标签的数据进行微调，目标是学习特定任务的语言表示。
# Bert的预训练
## Masked LM（MLM）
Masked LM是Bert的创新点之一。Masked LM是指在预训练时，对于一个输入的次元序列，有15%概率会将一个次元进行替换（用作掩码），第一个词元和分隔词元不进行替换（100个单词中有15个单词被mask）。对于替换的词元（掩码），有80%的概率替换成[MASK]，有10%的概率替换成其他的词，有10%的概率不进行替换。比如：
80%：my dog is hairy -> my dog is [MASK]
10%：my dog is hairy -> my dog is apple
10%：my dog is hairy -> my dog is hairy
这样能够使得模型在预训练时，能够聚焦于句子的大部分词，而不是全部词。
## Next Sentence Prediction（NSP）
Next Sentence Prediction是指在预训练时，对于一个输入的句子对，有50%的概率是真实的句子对，有50%的概率是随机的句子对。模型的目标是判断这两个句子是否是真实的句子对。
比如：
正例，句子对是真实的：
Input = [CLS] the man went to [MASK] store [SEP] he bought a gallon [MASK] milk [SEP]
Label = IsNext
负例，句子对是随机的：
Input = [CLS] the man [MASK] to the store [SEP] penguin [MASK] are flight ##less birds [SEP]
Label = NotNext
其中[SEP]标签是句子的分隔符，[CLS]标签用于类别预测（是正例还是负例），[MASK]标签是句子的掩码标志。
这样能够使得模型在预训练时，能够学习到句子之间的关系。因为在QA和自然语言推理里面，它们都是面向一个句子对，这样做能够极大的提高QA和自然语言推理的效果。
## bert的输入
bert input = token embedding + segment embedding + position embedding
其中token embedding是词的向量表示，segment embedding是句子的向量表示，position embedding是词的位置向量表示。bert中position embedding与transformer不同，transformer是使用sin和cos函数，而bert是使用随机初始化，然后让模型学习的方式。
## bert的微调
bert的微调是指在预训练好的bert模型上，进行特定任务的训练。比如在情感分析任务上，我们可以使用bert模型，然后在此基础上进行微调，得到一个情感分析模型。
### bert的下游任务
1. 句子对分类任务
2. 单个句子分类任务
3. 问答任务
4. 序列标注任务
### bert的微调策略
比如微博文本情感分析
1. 在大量通用语料上预训练——一般直接使用预训练好的bert模型
2. 在相同领域上继续训练（Domain transfer）——在微博文本上继续训练
3. 在任务相关的数据上继续训练（Task-specific）——在微博文本情感分析数据上继续训练
4. 在任务相关的数据上微调（Fine-tuning）——在微博文本情感分析数据上微调

Linear probe：固定encoder参数，学习linear层
Fine-tuning：整个模型包括encoder一起学习

