## 【Task2 GBDT算法梳理】2天

1. GBDT是一种迭代的决策树算法，该算法由多棵决策树组成，所有树的结论累加起来做最终的答案。
### 1、前向分布算法
   在给定训练数据和损失函数L(y,f(x))的条件下，学习加法模型f(x)成为经验风险极小化(即损失函数极小化问题) ，前向分步算法求解这一优化问题的想法是：由于学习的是加法模型，如果能从前向后每一步只学习一个基函数及其系数，逐步逼近优化目标函数式。
###  2、负梯度拟合
   Freidman提出了梯度提升算法：利用最速下降的近似方法，即利用损失函数的负梯度在当前模型的值，作为回归问题中提升树算法的残差的近似值，拟合一个回归树。
### 3、损失函数
   模型的不靠谱程度，损失函数越大，说明模型越容易出错。
### 4、回归
   回归树总体流程类似于分类树，区别在于，回归树的每一个节点都会得一个预测值，以年龄为例，该预测值等于属于这个节点的所有人年龄的平均值。分枝时穷举每一个feature的每个阈值找最好的分割点，但衡量最好的标准不再是最大熵，而是最小化平方误差。也就是被预测出错的人数越多，错的越离谱，平方误差就越大，通过最小化平方误差能够找到最可靠的分枝依据。分枝直到每个叶子节点上人的年龄都唯一或者达到预设的终止条件(如叶子个数上限)，若最终叶子节点上人的年龄不唯一，则以该节点上所有人的平均年龄做为该叶子节点的预测年龄。
###  5、二分类 多分类
   对于二元GBDT，如果用类似于逻辑回归的对数似然损失函数，则损失函数为：

   其中，y∈{−1,+1}。此时的负梯度误差是 ：

   对于生成的决策树，我们各个叶子节点的最佳残差拟合值为 ：

   由于上式比较难优化，我们一般使用近似值代替：

   除了负梯度计算和叶子节点的最佳残差拟合的线性搜索，二元GBDT分类和GBDT回归算法过程相同。

### 优缺点

### sklearn参数
   class sklearn.ensemble.GradientBoostingClassifier(loss=’deviance’, learning_rate=0.1, n_estimators=100, subsample=1.0, criterion=’friedman_mse’, min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_depth=3, min_impurity_decrease=0.0, min_impurity_split=None, init=None, random_state=None, max_features=None, verbose=0, max_leaf_nodes=None, warm_start=False, presort=’auto’, validation_fraction=0.1, n_iter_no_change=None, tol=0.0001)

### 应用场景
   GBDT几乎可用于所有回归问题（线性/非线性），相对logistic regression仅能用于线性回归，GBDT的适用面非常广。亦可用于二分类问题（设定阈值，大于阈值为正例，反之为负例）。

**参考**：

西瓜书
          
cs229吴恩达机器学习课程
           
李航统计学习
           
谷歌搜索

公式推导参考：http://t.cn/EJ4F9Q0

[参考答案](./../参考答案)