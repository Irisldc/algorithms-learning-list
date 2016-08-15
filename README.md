# algorithms-learning-list
the learning list of basic data mining algorithms

#Aprior algorithm
http://www.cnblogs.com/fengfenggirl/p/associate_apriori.html  

关联规则挖掘的定义：给定一个交易数据集T，找出其中所有支持度support >= min_support、自信度confidence >= min_confidence的关联规则。  
穷举法：测试集合中的每一个组合，并判断是否满足条件；一个元素个数为n的项集的组合个数为2^n-1(除去空集)，所需要的时间复杂度明显为O(2^N)。为了减少频繁项集的生成时间，我们应该尽早的消除一些完全不可能是频繁项集的集合，Apriori的两条定律就是干这事的。 

Apriori定律1)：如果一个集合是频繁项集，则它的所有子集都是频繁项集。举例：假设一个集合{A,B}是频繁项集，即A、B同时出现在一条记录的次数大于等于最小支持度min_support，则它的子集{A},{B}出现次数必定大于等于min_support，即它的子集都是频繁项集。  
Apriori定律2)：如果一个集合不是频繁项集，则它的所有超集都不是频繁项集。举例：假设集合{A}不是频繁项集，即A出现的次数小于min_support，则它的任何超集如{A,B}出现的次数必定小于min_support，因此其超集必定也不是频繁项集。  

在Apriori算法中，寻找最大项目集(频繁项集)的基本思想是：算法需要对数据集进行多步处理。第一步，简单统计所有含一个元素项目集出现的频数，并找出那些不小于最小支持度的项目集，即一维最大项目集。从第二步开始循环处理直到再没有最大项目集生成。循环过程是：第k步中，根据第k-1步生成的(k-1)维最大项目集产生k维侯选项目集，然后对数据库进行搜索，得到侯选项目集的项集支持度，与最小支持度进行比较，从而找到k维最大项目集。  

注意在生成高维频繁项集时，一定要看k-1次低维项集是不是频繁项集，如果不是，这个高维项集就也不是频繁项集，此处应用的是Apriori定律2

算法缺点：
(1)在每一步产生侯选项目集时循环产生的组合过多，没有排除不应该参与组合的元素;  (2)每次计算项集的支持度时，都对数据库D中的全部记录进行了一遍扫描比较，如果是一个大型的数据库的话，这种扫描比较会大大增加计算机系统的I/O开销。

#F-P Growth: Frequent Pattern Tree
http://www.360doc.com/content/09/1112/18/96202_8895093.shtml
http://blog.sina.com.cn/s/blog_68ffc7a40100uebg.html  

该算法只进行2次数据库扫描且它不使用侯选集，直接压缩数据库成一个频繁模式树，最后通过这棵树生成关联规则。研究表明它比Apriori算法大约快一个数量级。  

FP-growth算法是一种不产生候选模式而采用频繁模式增长的方法挖掘频繁模式的算法。算法只需要扫描2次数据库：第一次扫描数据库，得到1维频繁项集；第二次扫描数据库，利用1维频繁项集过滤数据库中的非频繁项，同时生成FP树。由于FP树蕴涵了所有的频繁项集，其后的频繁项集的挖掘只需要在FP树上进行。  

代码实现：https://github.com/enaeseth/python-fp-growth

# Naive Bayes
http://www.cnblogs.com/leoo2sk/archive/2010/09/17/naive-bayesian-classifier.html 
http://www.letiantian.me/2014-10-12-three-models-of-naive-nayes/  

通过条件概率P(A|B)得到条件概率P(B|A): P(B|A)=[P(A|B)P(B)]/P(A)  
朴素贝叶斯的思想基础是这样的：对于给出的待分类项，求解在此项出现的条件下各个类别出现的概率，哪个最大，就认为此待分类项属于哪个类别。  
特征是连续值时，一般假设特征值服从高斯分布。  
当特征值没有在训练集中出现时，则需使用laplace平滑
