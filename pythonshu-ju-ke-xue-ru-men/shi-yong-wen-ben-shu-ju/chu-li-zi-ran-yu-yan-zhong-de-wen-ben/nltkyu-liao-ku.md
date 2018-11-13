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

结果：

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

结果：

```
feline mammal usually having thick soft fur and no ability to roar: domestic cats; wildcats
********************
an informal term for a youth or man
```

同义词可以具有以上义词（含义较为抽象的同义词）和下义词（含义较为具体的同义词），这些功能使得同义词看起来像具有子类和超类的面向对象（OOP）类。

```
import nltk

wn=nltk.corpus.wordnet
print(wn.synset("cat.n.01").hypernyms())
print("*"*20)
print(wn.synset("cat.n.01").hyponyms())
```

结果：

```
[Synset('feline.n.01')]
********************
[Synset('domestic_cat.n.01'), Synset('wildcat.n.03')]
```

最后，可以使用WordNet来计算两个同义词组之间的语义相似性。相似度是范围【0...1】中的双精度数。如果相似性为0，则同义词是无关性的；如果相似性为1，则同义词是完全同义词。

```
from nltk.corpus import  wordnet as wn

x=wn.synset("cat.n.01")
y=wn.synset("lynx.n.01")

print(x.path_similarity(y))
```

结果：

```
0.04
```

上面提到的是两个词之间的相似性，那么任意两个词之间的相似性如何分析呢？让我们来看看【dog】和【cat】的所有同义词，并找到语义最接近的定义：

```
import nltk

wn=nltk.corpus.wordnet
result=[simxy.definition() for simxy in
        max((x.path_similarity(y),x,y)
            for x in wn.synsets('cat')
            for y in wn.synsets('dog')
            if x.path_similarity(y) #确保synsets是相关的
            )[1:]]

print(result)
```

结果：

```
['an informal term for a youth or man', 'informal term for a man']
```

真是太神奇了！

