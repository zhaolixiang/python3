$$x = y$$根据经验，所有潜在的可用数据中，约80%的数据是非结构化的，其中包括音频、视频、图像和用自然语言编写的文本。自然语言中的文本没有标签、分隔符和数据类型，但它仍然是丰富的信息源。有时候我们想要知道某些词是否出现在文本中及出现的频率（**词句标记**），文本属于什么类别（**文本分类**），它传达了正面的还是负面的消息（**情感分析**），文本中提到的人和物（**实物提取**），等等。如果只是一两个文本，我们尚且可以靠肉眼来读取和处理，但对于大规模的文本分析，就必须借助于自然语言处理（NLP）。

Python的nltk模块（自然语言功能工具包）中实现了很多NLP的功能。该模块围绕语料库（字词和表达式的集合）、函数和算法进行组织。

# NLTK语料库

语料库是单词或表达式的结构化或非结构化集合。所有NLTK语料库都存储在模块nltk.corpus中，下面是一些例子：

* gutenberg：包含来自古登堡计划的十八个英语文本，例如《白鲸》和《圣经》
* names：8000名男性和女性名字的列表。
* words：235000个最常用的英语单词和表单的列表。
* stopwords：以十四种语言列出的停用词。大多是的文本分析都要删除停用词，因此通常他们对于文本的理解不会带来新的信息。
* cmudict：诞生在卡内基梅隆大学的发言字典，有134000多个条目。cmudict.entries\(\)中的每个条目都是一个单纯和一个音节列表组成的元组，同一个词可能有几种不同的发音。这个语库可用于识别同音词（soundalikes）

nltk.corpus.wordnet对象是另一个语料库的接口：一个在线语义词网络WorldNet（需要访问网络）。该网络是用词性和序列号标记的同义词集合：

```
import nltk
wn=nltk.corpus.wordnet
print(wn.synsets("cat"))
```

结果

```
[Synset('cat.n.01'), Synset('guy.n.01'), Synset('cat.n.03'), Synset('kat.n.01'), Synset('cat-o'-nine-tails.n.01'), Synset('caterpillar.n.02'), Synset('big_cat.n.01'), Synset('computerized_tomography.n.01'), Synset('cat.v.01'), Synset('vomit.v.01')]
```

可以查找每个同义词的定义，这可能是一个意想不到的功能：

```
import nltk

wn=nltk.corpus.wordnet
print(wn.synset("cat.n.01").definition())
print("*"*20)
print(wn.synset("cat.n.02").definition())
```

结果

```
feline mammal usually having thick soft fur and no ability to roar: domestic cats; wildcats
********************
an informal term for a youth or man
```

同义词可以具有以上义词（含义较为抽象的同义词）和下义词（含义较为具体的同义词），这些功能使得同义词看起来像具有子类和超类的面向对象（OOP）类。

```

```

最后，可以使用WordNet来计算两个同义词组之间的语义相似性。相似度是范围【0...1】中的双精度数。如果相似性为0，则同义词是无关性的；如果相似性为1，则同义词是完全同义词。

```

```

上面提到的是两个词之间的相似性，那么任意两个词之间的相似性如何分析呢？让我们来看看【dog】和【cat】的所有同义词，并找到语义最接近的定义：

```

```

真是太神奇了！

# 通过PlaintextCorpusReader创建自己的语料库

除了使用标准语料库，还可以通过PlaintextCorpusReader创建自己的语料库。PlaintextCorpusReader会在根目录中查找与glob模式匹配的文件：

```
myCorpus=nltk.corpus.PlaintextCorpusReader(root,glob)
```

函数fileids\(\)返回包含在新创建的语料库中的文件列表。函数raw\(\)返回语料库中原始的【原始文本】。函数sents\(\)返回所有句子的列表。函数words\(\)返回所有的单词列表。

# 规范化

规范化是用自然语言的表达式对文本进行整理，以便进一步处理的过程。规范化通常包括以下步骤：

### 1、分词（将文本切分成单词）。

NLTK提供了两个简单的和两个较为高级的分词器。句子切分器返回一个列表，列表元素为句子字符串。其它所有分词器返回的都是一个单词列表：

* work\_tokenize\(text\)：切分单词
* sent\_tokenize\(text\)：切分句子
* regexp\_tokenize\(text,re\)：基于正则表达式的分词，参数re是描述有效字的正则表达式

根据分词器的质量和句子的标点结构，有些【词】可以包含非字母的字符。对于严重依赖于标点符号的分析人物（例如通过表情符号的情感分析），需要用到更为高级的wordPunctcTokenizer。下面是WorkPuncTokenizer.tokenize\(\)和word\_token\(\)对相同文本解析的结果的对比：

```

```



