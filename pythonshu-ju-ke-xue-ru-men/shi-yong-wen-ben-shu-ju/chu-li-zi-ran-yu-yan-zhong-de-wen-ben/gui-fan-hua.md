### 规范化

规范化是用自然语言的表达式对文本进行整理，以便进一步处理的过程。规范化通常包括以下步骤：

##### 1、分词（将文本切分成单词）。

NLTK提供了两个简单的和两个较为高级的分词器。句子切分器返回一个列表，列表元素为句子字符串。其它所有分词器返回的都是一个单词列表：

* work\_tokenize\(text\)：切分单词
* sent\_tokenize\(text\)：切分句子
* regexp\_tokenize\(text,re\)：基于正则表达式的分词，参数re是描述有效字的正则表达式

根据分词器的质量和句子的标点结构，有些【词】可以包含非字母的字符。对于严重依赖于标点符号的分析人物（例如通过表情符号的情感分析），需要用到更为高级的wordPunctcTokenizer。下面是WorkPuncTokenizer.tokenize\(\)和word\_token\(\)对相同文本解析的结果的对比：

```
from nltk.tokenize import WordPunctTokenizer
import nltk
word_punct=WordPunctTokenizer()
text="}Help! :))) :[ ..... :D{"
result=word_punct.tokenize(text)
print(result)

print(nltk.word_tokenize(text))
```

结果：

```
['}', 'Help', '!', ':)))', ':[', '.....', ':', 'D', '{']
['}', 'Help', '!', ':', ')', ')', ')', ':', '[', '...', '..', ':', 'D', '{']
```

##### 2、将单词转换为大小写相同的字符（全部大写或小写）

##### 3、删除停用词。使用语料库中的停用词和其他特定的停用词列表作为参考。需要注意的是，停用词使用的是小写字母。语料库中并不存在【the】这个停用词，虽然它可定是停用词。

##### 4、词干化处理。NLTK提供了两个基本的词干分析器：较为保守的Porter和较为激进的Lancaster。由于其激进原则，Lancaster会产生更多的音形相近但含义不同的词干。两个分析器都有stem\(word\)，这个函数会返回所谓的词干。

> 对于英文而言，所谓的词干化处理主要包括把名词的复数形式变成单数，将其他时态变成基本形态等类似的处理。

```
import nltk

pstemmer=nltk.PorterStemmer()
lstemmer=nltk.LancasterStemmer()
print(pstemmer.stem("wonderful"))
print(lstemmer.stem("wonderful"))
```

结果：

```
wonder
wond
```

两个词干分析器的特点在于：仅将词干分析应用于单个单词，而不是完整短语。

##### 5、词形还原：一种速度更慢也更保守的词干分析机制。WordNetLemmatizer查找WordNet中算出的词干，仅接受单词或表格形式的词干。要使用lemmatizer，需要访问互联网，函数lemmatize\(word\)返回work表示的词的词元：

```
import nltk

lemmatizer=nltk.WordNetLemmatizer()
print(lemmatizer.lemmatize("wonderful"))
```

结果：

```
wonderful
```

另外，虽然在计数层面上，词性标记（POS标记）并不属于规范化的范畴，但它也是文本预处理中的重要步骤。函数nltk.pos\_tag\(text\)为文本中的每个单词分配一个词性标签，其中文本是一个单词列表。函数的返回值是一个元组列表，其中元组的第一个元素是原始单词，第二个元素是标签。

```
import nltk

print(nltk.pos_tag(["beautiful","world"]))
```

结果：

```
[('beautiful', 'JJ'), ('world', 'NN')]
```

### 下面的代码从文件index.html中找出10个最常用的非停用词的词干：

```
from bs4 import BeautifulSoup
from collections import Counter
from nltk.corpus import stopwords
from nltk import LancasterStemmer,word_tokenize

#创建一个新词干
ls=LancasterStemmer()

#读取文件并生成soup
with open("index.html") as infile:
    soup=BeautifulSoup(infile)
    
#提取并标记文本
words=word_tokenize(soup.text)

#转换为小写
words=[w.lower for w in words]

#删除停止单词，并分析剩余部分的词干
words=[ls.stem(w) for w in words if w not in stopwords.words("english") and w.isalnum]

#对词进行计数
freqs=Counter(words)
print(freqs.most_common(10))
```



