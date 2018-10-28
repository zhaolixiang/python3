常见的数据分析研究步骤通常与一般的科学发现顺序一致，

数据科学发现从【要解决的问题】和【要应用的分析方式】开始。最简单的分析类型是**描述性**的，通常使用一种可视化的形式给出数据集总量的描述。不论你接下来打算做什么，至少需要描述一下数据！在【探索性】数据分析的过程中，你需要尝试找出现有变量之间的相互关系。基于统计的【推断分析】可以帮助你利用手头上少量的数据样本，对更大的群体进行描述。【预测分析】是从过去的规律中预测未来。【因果分析】能找出相互影响的变量。最后，【机制性】数据分析准确揭示了一个变量如何影响另一个变量。

然而，分析结果的好坏依赖于数据的质量，因此引出如下问题：什么样的数据是理想的呢？在理想情况下，什么样的数据能够解决问题呢？另外，理想的数据集可能根本就不存在，或者是很难甚至不可能获取。对于这种情况，一个较小的或者特征不那么丰富的数据集还依然有用吗？

幸运的是，从Web或数据库获取原始数据并不难，有大量基于Python的工具可用于下载和解析这些数据。

应该注意到，完美的数据是不存在的。难免会遇到丢失值、异常值和其他【非标准项的【脏】数据。【脏】数据的例子包括：未来的生日期、负年龄、负体重，以及不合理的电子邮箱等等。因此，一旦获取了原始数据，接下来就是使用数据清洗工具和统计知识来正则化数据集。

完成上述处理后，就可以使用干净的数据，开展描述性和探索性分析。这一步的成果通常包括散列图、直方图、统计总结。他们赋予了你对数据独有的感觉：这是一种在后续研究中不可或缺的对数据集（尤其是针对多维数据集）的直观认识。

现在离现实预测只有一步之遥了。你手中的数据模型工具，在经过恰当的训练后，就可以实现预测功能。值得注意的是，不能忽视对模型的质量及其预测精度的评估！

至此，你可以摘掉统计学家和程序员的帽子，还上一顶领域专家的帽子了。你已经得到了一些成果，但它们称得上是领域内的重要成果吗？换句话说，是否有人关心这些成果，还有，这些成果带来了什么不一样的认知？设想一下，你被聘用为一名评论员，来评价自己的工作。你做的哪些是正确的，哪些是错误的？如果再给你一次机会，哪些工作你能做的更好或不同？你是否使用不同的数据，做出不同类型的分析，提出不同的问题，抑或建立一个不同的模型？一定有人会提出这些问题。提前进行思考，对你是大有裨益的。当你还沉浸在这些字里行间时，寻觅答案的征程已经开始。

最后，你必须完成一个报告，说明你处理数据的方式及理由、建立了什么模型、可能得出什么结论、可能做出什么预测。
