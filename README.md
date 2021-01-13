# AnomalyDetection
## 初识异常检测
### 1.概念：
**异常**一般是指与标准值（预期值）有偏离的样本点，也就是跟绝大部分数据“长的不一样” ；  
异常往往是“有价值”的事情，我们对异常的成因感兴趣 
### 2.类别：
- 有监督：数据集有标签
- 无监督：数据集无标签（异常检测往往是在无监督模式下进行的，获取的数据都是无标签的）
- 半监督：数据集只有单一类别（正例）有标签，没有异常实例参与训练
### 3.应用：
- 金融行业反欺诈、信用卡诈骗检测：把欺诈或者金融风险当作异常
- 罕见病检测：把罕见病当作异常，比如罕见早期的阿尔兹海默症
- 入侵检测：把网络流量中的入侵当作异常
- 机器故障检测：实时检测发现或预测机器故障
- 图结构、群体检测：比如检测疫情的爆发点等
### 4.方法：
#### （1）传统方法
- **基于统计学的方法**：拟合给定数据集的生成模型，并对它进行学习，该模型低频率区域的对象为异常数据，把他们作为异常点。（正常的数据对象符合一定的数据模型，而不遵守该模型的数据是异常的）  
[数据集正态分布图](https://img-blog.csdnimg.cn/img_convert/7bf7f5801da27215e37171c513a765a8.png)  
如图所示，数据集符合正态分布，小于6的数据点都可以被视为异常点。
更多可参考[统计学方法](https://www.cnblogs.com/bigmonkey/p/11431468.html)
- **线性模型**：如PCA,对数据集进行降维，计算特征向量，降维后的数据在很大程度上保留了原始数据的特征，异常样本点距离特征向量的距离比较远，对这个距离设置一个阈值，超过此阈值的数据点被视为异常点。  
- **基于相似度的方法**：此方法适用于数据点`聚集程度高、离群点较少`的情况，需要对每个数据点进行相应计算，不适用于数据量大、维度高的数据。
	-  **基于集群的检测**：如DBSCAN等聚类方法，在聚类时，异常点不属于任何一个簇。该方法将数据点划分为一个个对应的簇，哪些不能被归为某一个簇的数据点被视为异常点。这类算法对簇个数的选择高度敏感，数量选择不当可能造成较多正常值被划为离群点或成小簇的离群点被归为正常。因此对于每一个数据集需要设置特定的参数，才可以保证聚类的效果，在数据集之间的通用性较差。聚类的主要目的通常是为了寻找成簇的数据，而将异常值和噪声一同作为无价值的数据而忽略或丢弃，在专门的异常点检测中使用较少。？
	-  **基于距离的度量方法**：如KNN方法，异常点到其临近的前K个点的距离比正常数据点要大。k近邻算法的基本思路是对每一个点，计算其与最近k个相邻点的距离，通过距离的大小来判断它是否为离群点。在这里，离群距离大小对k的取值高度敏感。如果k太小（例如1），则少量的邻近离群点可能导致较低的离群点得分；如果k太大，则点数少于k的簇中所有的对象可能都成了离群点。为了使模型更加稳定，距离值的计算通常使用k个最近邻的平平均距离。
	-  **基于密度的度量方法**：如LOF(局部离群因子)方法，异常点周围点的密度较低。相对于其邻居的局部密度偏差而不是距离来进行度量。它将相邻点之间的距离进一步转化为“邻域”，从而得到邻域中点的数量（即密度），认为密度远低于其邻居的样本为异常值。
#### （2）集成方法  
   集成算法是将多个基检测器的输出结合起来，一些算法在数据集的某些子集上表现很好，一些算法在其他子集上表现很好，然后集合起来，使得输出更具鲁棒性。  
   **孤立森林**：孤立森林是用一个随机超平面切割数据空间，切一次可以生成两个子空间。然后继续用随机超平面来切割每个子空间并循环，直到每个子空间只有一个数据点为止。直观上来讲，那些具有高密度的簇需要被切很多次才会将其分离，而那些低密度的点很快就被单独分配到一个子空间了。孤立森林认为这些很快被孤立的点就是异常点。  
#### （3）机器学习方法
   在有标签的情况下，可以使用树模型（gbdt,xgboost等）进行分类，缺点是异常检测场景下数据标签是不均衡的，但是利用机器学习算法的好处是可以构造不同特征
### 5.常用库：PyOD、sklearn、TODS
- **PyOD**:2019年开发的专门用于异常检测的库，包含30多张算法，从经典模型到深度学习模型一应俱全，用法和Sklearn的一样。  
- **TODS**:与PyOD类似，包含多种时间序列上的异常检测。
### 6.常用算法
#### 6.1基于统计学的方法:他们假定正常的数据对象由一个统计模型产生
- **参数方法**:*假定*数据符合某一参数的分布，该分布的概率密度给出![](https://latex.codecogs.com/gif.latex?f(x,\Theta&space;)),将数据点带入到概率密度公式，得出该数据点符合该分布的概率，概率越小，越不符合该分布，是异常点的可能性就越大
- 非参数方法
- HBOS
