# Logstic regression

Q1：***逻辑回归适合用于解决哪些问题？***

- 逻辑回归可用于输出或因变量为分类或二元的分类问题。但是，为了正确实现逻辑回归，数据集还必须满足以下属性：
    - 自变量之间不应该有很高的相关性。换句话说，预测变量应该相互独立。
    - logit结果和每个预测变量之间应该存在线性关系。logit函数为logit(p) = log(p/(1-p))，其中是p结果的概率。
    - 样本量必须很大。有多大取决于模型的独立变量的数量。
- 当满足上述所有要求时，可以使用逻辑回归。

Q2：***为什么逻辑回归被称为回归而不是分类？***

- 虽然我们在逻辑回归中的目标任务是分类，但*逻辑回归实际上*只是预测了概率（或 logit 形式的对数优势比）。通过加入门限，逻辑回归根据预测的概率与门限的比较结果做出分类

Q3：*****为什么我们不使用均方误差作为逻辑回归中的成本函数？*****

- 逻辑回归使用mse为损失函数时，是非凸的损失函数，不方便优化；而对数损失(二元交叉熵损失)能保证损失函数为凸函数，优化到全局最低点。
- 另外，逻辑回归的交叉熵损失函数也是可以通过最大似然估计推导得出的。

Q4：***逻辑回归和线性回归的区别？***

- 区别如下表

| 区别 | 线性回归 | 逻辑回归 |
| --- | --- | --- |
| 算法类型 | 回归问题 | 分类问题 |
| 损失函数 | MSE均方误差 | 交叉熵 |
| 表现形式 | 单纯的线性模型 | 在线性回归基础上加入了非线性映射函数sigmoid |

Q5：***逻辑回归一般用来寻找两个类之间的线性决策边界，它可以解决非线性分类问题里吗，如
$x_{1}^{2}+x_{2}=0$这种非线性超平面的分类***

- 可以，但需要使用核函数映射技巧将原始的特征做非线性变换；如下图，将二维数据映射至三维后，可以转换为一个易于求解的线性分类问题，再用逻辑回归求解。

![Untitled](Untitled%2031.png)

Q6：***当分类数据中有大量异常值时，比较SVM和Logistic 回归方法***

- LR对异常值敏感，LR在寻找分类决策面时，需要考虑空间中所有数据点
- SVM对异常值不敏感，SVM 只考虑局部的边界线附近的点（支持向量）
- 因此SVM模型的抗噪声能力更强

Q7：***逻辑回归的空间复杂度分析***

- 在训练逻辑回归模型期间，我们需要在内存中存储四样东西：x、y、w 和 b。由于b 是一个常量，所以存储b 只需1 步，即O(1) 操作。x 和 y 分别是 (nxd) 和 (nx 1) 阶的两个矩阵。存储这两个矩阵需要 O(nd + n) 步。最后，w 是一个大小为 d 的向量。将其存储在内存中需要 O(d) 步。因此，训练时的空间复杂度为 O(nd + n +d)。
- 在训练模型之后，我们只需要保留在内存中的是 w。我们只需要执行 wx+b 来对点进行分类。因此，运行时的空间复杂度为 d 量级，即 O(d)。

Q8：***比较决策树和逻辑回归模型***

- 决策边界：决策树将空间平分为越来越小的区域，而逻辑回归拟合一条线将空间精确地一分为二；如下图，树可以更好地捕获划分，从而导致更好的分类性能。然而，当类别没有很好地分离时，树很容易过度拟合训练数据，因此 Logistic 回归的简单线性边界泛化效果更好。

![Untitled](Untitled%2032.png)

![Untitled](Untitled%2033.png)

Q9：***比较朴素贝叶斯和逻辑回归***

- 朴素贝叶斯是一种生成模型，而 LR 是一种判别模型。
- 朴素贝叶斯适用于小型数据集，LR在大数据集上效果更好
- 当特征中存在相关性时，LR 比朴素贝叶斯表现更好，因为朴素贝叶斯期望所有特征都是独立的。

引用：[https://towardsdatascience.com/comparative-study-on-classic-machine-learning-algorithms-24f9ff6ab222](https://towardsdatascience.com/comparative-study-on-classic-machine-learning-algorithms-24f9ff6ab222)

Q10：***逻辑回归可以用于不平衡分类问题吗？***

- Logistic回归不直接支持不平衡分类；因为LR依赖于所有数据；
- 如果要处理不平衡分类问题，必须修改用于拟合逻辑回归模型的训练算法，以考虑数据偏斜的分布。这可以通过指定类加权配置来实现，该类加权配置用于影响训练期间逻辑回归系数的更新量。加权，可以减少模型对数据量大的类别产生错误时的惩罚，同时，对数据量小的类别，则施加更多惩罚。该方法被称为**加权逻辑回归。**

引用：[https://stats.stackexchange.com/](https://stats.stackexchange.com/)

Q11：***阐述SVM相对逻辑回归的优点***

- SVM根据支持向量确定分类平面，受数据中的异常点影响小；
- SVM转化为对偶问题后，分类只需要计算与少数几个支持向量的距离，这个在进行复杂核函数计算时优势很明显，能够大大简化模型和计算量；因此SVM更适合处理高维特征

Q12：***如何避免逻辑回归模型中的过拟合***

- 添加正则化项，L1或L2正则化，一般使用L2正则化；一开始并不加入正则项，而是在训练好LR模型后，如果各个变量的w有极大值，且与业务逻辑不符合时，再添加正则化重新训练
- 控制特征数量；将相关性强的特征去除

引用：[https://www.bogotobogo.com/](https://www.bogotobogo.com/)

Q13：*****为什么逻辑回归被认为是线性模型？*****

- 如果用于计算预测的特征变换是特征的 **线性组合** ，则模型被认为是线性的
- 逻辑回归被认作线性模型，是因为逻辑回归模型的特征在响应变量的**对数几率方面是线性的；逻辑回归的公式**
    
    $$
    p=\frac{1}{1+e^{-(w^{T}x+b}))}
    $$
    
    可以表示为：
    
    $$
    w^{T}x+b=log(\frac{p}{1-p})
    $$
    
    其中，$log(\frac{p}{1-p})$被称为对数几率，表示事件发生与不发生的比率；逻辑回归实际上是在对对数几率建模，对数几率关于特征是线性的
    

引用：[http://www.ciaburro.it/why-is-logistic-regression-considered-a-linear-model/](http://www.ciaburro.it/why-is-logistic-regression-considered-a-linear-model/)