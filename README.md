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

#DFS:深度优先搜索 vs BFS:广度优先搜索 
http://www.cnblogs.com/skywang12345/p/3711483.html  
http://www.cnblogs.com/daoluanxiaozi/archive/2012/05/18/2507212.html  

DFS: 可以被形象的描述为“打破沙锅问到底”，具体一点就是访问一个顶点之后，我继而访问它的下一个邻接的顶点，如此往复，直到当前顶点一被访问或者它不存在邻接的顶点  
BFS:可以被形象的描述为“浅尝辄止”，具体一点就是每个顶点只访问它的邻接节点（如果它的邻接节点没有被访问）并且记录这个邻接节点，当访问完它的邻接节点之后就结束这个顶点的访问

# Regularization
http://blog.csdn.net/zouxy09/article/details/24971995  

不同machine learning模型的loss function不同  
最小二乘: square loss  
SVM: Hinge loss  
GBDT: exp-loss  
logistic regression: log-loss  
规则化函数Ω(w)也有很多种选择，一般是模型复杂度的单调递增函数，模型越复杂，规则化值就越大，常用L0,L1,L2  
L0: L0范数是指向量中非0的元素的个数。如果我们用L0范数来规则化一个参数矩阵W的话，就是希望W的大部分元素都是0,换句话说，让参数W是稀疏的。  
L1(Lasso): L1范数是指向量中各个元素绝对值之和，也有个美称叫“稀疏规则算子”（Lasso regularization)  
总结：L1范数和L0范数可以实现稀疏，L1因具有比L0更好的优化求解特性而被广泛应用  
为何青睐于将特征变稀疏：  
1）可以实现特征的自动选择：稀疏规则化算子会学习地去掉这些没有信息的特征，也就是把这些特征对应的权重置为0  
2）使模型更容易理解，可以筛选出哪些特征是决定性的  

L2(Ridge): L2范数是指向量各元素的平方和然后求平方根。  
L2范数不但可以防止过拟合，还可以让我们的优化求解变得稳定和快速

#k-fold validation
http://blog.csdn.net/holybin/article/details/27185659  
将原始数据分成K组（一般是均分），将每个子集数据分别做一次验证集，其余的K-1组子集数据作为训练集，这样会得到K个模型，用这K个模型最终的验证集的分类准确率的平均数作为此K-CV下分类器的性能指标。K一般大于等于2，实际操作时一般从3开始取，只有在原始数据集合数据量小的时候才会尝试取2。K-CV可以有效的避免过学习以及欠学习状态的发生，最后得到的结果也比较具有说服性。

#Generative model生成模型 vs. Discriminative model判别模型
http://dataunion.org/8743.html  

观察值：o, 模型：q  
p(o|q): Generative model生成模型，估计的是联合概率分布（joint probability distribution），p(class, context)=p(class|context)*p(context)  
包括：Gaussians, Naive Bayes,HMM(隐马尔科夫),Markov random fields, Restricted Boltzmann Machine, LDA(Latent Dirichlet Allocation)

p(q|o):Discriminative model, 又可以称为条件模型，或条件概率模型。估计的是条件概率分布(conditional distribution)， p(class|context)  
包括：Logistic regression, SVM, Boosting, traditional neural networks, Conditional random fields(CRF)
