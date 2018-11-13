# 通过PlaintextCorpusReader创建自己的语料库

除了使用标准语料库，还可以通过PlaintextCorpusReader创建自己的语料库。PlaintextCorpusReader会在根目录中查找与glob模式匹配的文件：

```
myCorpus=nltk.corpus.PlaintextCorpusReader(root,glob)
```

函数fileids\(\)返回包含在新创建的语料库中的文件列表。函数raw\(\)返回语料库中原始的【原始文本】。函数sents\(\)返回所有句子的列表。函数words\(\)返回所有的单词列表。

