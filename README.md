# algorithms-learning-list
the learning list of basic data mining algorithms

#Aprior algorithm
http://www.cnblogs.com/fengfenggirl/p/associate_apriori.html
关联规则挖掘的定义：给定一个交易数据集T，找出其中所有支持度support >= min_support、自信度confidence >= min_confidence的关联规则。＜/br＞
穷举法：测试集合中的每一个组合，并判断是否满足条件；一个元素个数为n的项集的组合个数为2^n-1(除去空集)，所需要的时间复杂度明显为O(2^N)
为了减少频繁项集的生成时间，我们应该尽早的消除一些完全不可能是频繁项集的集合，Apriori的两条定律就是干这事的。
Apriori定律1)：如果一个集合是频繁项集，则它的所有子集都是频繁项集。举例：假设一个集合{A,B}是频繁项集，即A、B同时出现在一条记录的次数大于等于最小支持度min_support，则它的子集{A},{B}出现次数必定大于等于min_support，即它的子集都是频繁项集。
Apriori定律2)：如果一个集合不是频繁项集，则它的所有超集都不是频繁项集。举例：假设集合{A}不是频繁项集，即A出现的次数小于min_support，则它的任何超集如{A,B}出现的次数必定小于min_support，因此其超集必定也不是频繁项集。
