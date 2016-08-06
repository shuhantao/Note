# Chapter 1 绪论
#### 1.2 基本术语
- 分类和回归的区别  分类是预测离散值 回归是预测连续值 
- 根据训练数据是否拥有标记信息 分为有监督和无监督  回归是前者代表 聚类是后者代表
- 通常假设每个样本都是独立同分布 iid

#### 1.3 假设空间
我们可以把学习过程看作一个在所有假设组成的空间中搜索的过程  
假设空间: 所有可能的假设组成的空间 即指所有搜索的可能 自顶向下  
版本空间: 与训练集一致的假设集合 搜索方法 

#### 1.4 归纳偏好
归纳偏好是指在选择模型的时候进行的选择  
奥卡姆剃刀法则: 若存在多个假设与观察一致 选择最简单的那一个  
没有免费的午餐定理(NFL): 在问题出现的机会相同或者所有问题同等重要下学习算法的期望性相同。   
告诉我们谈论算法必须针对问题

# Chapter 2 模型评估与选择
#### 2.1 经验误差与过拟合
- 训练误差(经验误差): 学习器在训练集上的误差  
- 泛华误差: 学习器在新样本上的误差  
- 欠拟合和过拟合 欠拟合比较容易客服 如在决策树中加扩展分支 在神经网络中增加训练轮数  过拟合无法避免 只能缓解

#### 2.2 评估方法
1. 留出法  
  将数据集分成两个互斥的集合  一个用于训练集S 一个为测试集T  
  分层采样 保证训练数据和测试数据中正例和反例比例相同  
  分隔合理 例如分隔时位置 对于某个参数  
  多次平均

2. 交叉验证法  
  将数据集分成k个大小相似的子集 每个子集尽量保证数据分布一致性 每次用k-1个子集的并进行训练 用剩下一个进程测验 那么可以测验k次 取这k次的平均  
  与此相关的 留一法 每次只取一个样本测验 只能用于样本数量不大的情况下

3. 自助法(bootstraping)
  每次从原样本随机挑选一个样本  持续m次即得到一个大小为m的样本 在m次取样中 有
  每次从原样本随机挑选一个样本  持续m次即得到一个大小为m的样本 在m次取样中 有36.8%的与样本不同 我们就可以将这m个样本做训练集 它和原样本的差集作为测试集合 

#### 2.3 性能度量
- 回归任务最常用的是均方误差   
- 查准率 precision 查全率 recall P = TP/(TP+FP)  R=TP/(TP+FN)   
  查准率和查全率通常是一对矛盾的度量  一者高时另外一个通常会低  
- PR曲线   
  若要比较两个学习器的能力 那么可以比较他们的面积   
  平衡点 BEP 当查准率=查全率的点  
- F1 和Fbeta  对查准率和查全率有不同的要求的时候用Fbeta  Fbeta=(1+beta^2)*P*R/((beta^2)*P+R)   
  当beta>1 时对查全率有更大的影响  当beta<1时对查准率有更大影响
- 若有多次训练 则可以得到一个二维混淆矩阵 那么可以得到平均的P和R 然后在利用平均的PR 得到平均的F1
- ROC和AUC  
  在对一个实数值进行预测的时候 通常会将其放在[0,1]中 如果大于某个阈值则为正例 那么就可以通过确定阈值进行确定 如果更重视差准 那么则放置较前  反之相同   
  ROC ROC曲线纵轴为TPR 真正例率 横轴为FPR 假正率  
  与PR曲线相同 面积越大则学习器越佳 面积叫AUC  
  形式化上AUC是确定预测的质量 定义loss = 1/(m<sup>+</sup>m<sup>-</sup>) (sum(is(f(x<sup>+</sup>)>f(x<sup>-</sup>)))+sum(is(f(x<sup>+</sup>)=f(x<sup>-</sup>)))/2)  AUC = 1-loss
- 非均等代价  
  由于有时候将正例判成负例和将负例判成正例说带来的代价不同，故可以将他们赋权值 赋权之后会得到新的错误率以及ROC曲线

#### 2.4 比较检验
机器学习重点比较检验大多为假设检验
- 二分检验 在机器学习中，判断错误的清空可以用二项分布模型 对应当可以用二项检验来判断在某个置信区间下 学习器的泛化错误情况
- 对同一个学习器的多次建业可以用t检验来判断泛化性能  利用t分布及计算得到的方差和均值 可以得到在一定置信区间下的泛化错误情况
- 交叉t检验 针对两个学习器 对于两个学习器 我们主要要判断两个学习器的泛化学习能力是否有显著差异 我们用两个学习器的插值的绝对值当做变量 并判断他在k-1的自由度下的t分布   
  在这里有x折的概念  由于害怕实验训练集合有一定程度的重叠故错误率不独立 故引入了x折  x折代表每x次 需要计算这x次的方差
- NcNemar检验 同样是针对两个学习器 两个学习器对同一测试样例 可能有4种结果关于是否测试正确 其中一方错误另一方正确的概率应该是相同的 在这里这两个概率的差应该服从正态分布 均值为1 那么可以设计对应的卡方分布 根据卡方分布判断是否有显著差异
- Friedman检验和Nemenyi检验   
  Friedman检验是对多个学习器的检验  将他们的测试性能进行排序 若有相同的平分序值 得到一个序值表 之后得到平均序值  
  平均序值服从正态分布 均值为(k+1)/2 方差为(k^2-1)/12 可以设置卡方分布变量  
  也可以用F分布代替卡方分布 F = (N-1)*x/(N(k-1)*x^2) x代表卡方分布  他服从k-1 和(k-1)(N-1) 的F分布  
  这可以判断所有算法是否显著相同 若有不同则可以用Nemenyi进行后续检验  
  CD = q(k*(k+1)/(6N))^0.5  若两个算法的平均序值超过CD 那么可以拒绝两个算法性能相同这个假设
#### 2.5 偏差和方差
通过公司据算可以看到泛化误差是偏差 方差 噪声的和  
偏差和方差是相互冲突的一对量  偏差-方差窘境