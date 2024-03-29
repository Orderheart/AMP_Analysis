结合NLP模型，建立统一的AMP识别管道。我们应用NLP算法构建AMP预测模型，包括5个NNMs，分为3类(图2b)。

我们的基础模型是基于Veltri等人的26，一个以长短期记忆(LSTM)为核心层的NNM，已经被证明对AMP识别有效。

第二个模型用注意力层取代了LSTM层，从而产生了ATT模型。

最后，我们包含了一个基于变压器的双向编码器表示(BERT)模型27。

使用独立的数据集对NLP模型的超参数进行测试和筛选(方法)，所有模型在训练过程中都快速收敛(扩展数据图1b)。



![image-20240129011918708](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20240129011918708.png)

![image-20240129012126172](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20240129012126172.png)

我们发现，不同模型识别的真阳性(TP)和假阳性(FP)序列的比例差异很大:

LSTM预测的FPs比BERT高56倍，

TPs比BERT高11%，

而ATT预测的FPs比BERT高46%，

TPs比BERT高5%(补充表1)。

这些差异并不是简单地反映了不同模型的不同敏感性，因为，当我们比较所有模型共享的TPs和FPs时(TPs: 1,678和FPs:3,981)，并在每个模型的最后一个隐藏层提取这些TPs和FPs的预测向量，只有不到0.3%的这些序列在五个模型之间的成对相关性是显著相关的(错误发现率(FDR)<0.05)(图2c和补充表2)。



由于它们的预测偏差是相互独立的，我们探索将这些不同的模型结合起来进一步提高精度并降低FPs。我们最终测试了各种模型组合的交集(2-5个模型)，并使用Precision, Recall和Area Under the Precision Recall Curve (AUPRC;方法)。通过AUPRC和这两个参数的排序，

发现精度最高的组合是三个模型的组合，准确率为91.31% (ATT、LSTM和BERT，与单一BERT模型的最佳性能相比，精度提高了约15%)。召回率达到83.32%，最高AUPRC为0.9244(图2d和补充表3)。

与使用相同测试数据集的其他现有AMP预测方法进行比较显示，我们的管道在AUPRC(图2e)和Precision(其他工具)方面超过了所有其他方法:27.21~2.67%)，其中4个工具预测的TP AMP比我们的预测高0.66 ~ 6.75%，而FPs比我们的管道高74 ~182倍(表1)。

这些结果表明，我们的管道结合了多个NLP模型，是一种从序列数据中识别AMP的稳健方法。