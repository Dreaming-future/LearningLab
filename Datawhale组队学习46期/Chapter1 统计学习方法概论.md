# 第1章 统计学习方法概论

1．统计学习是关于计算机基于数据构建概率统计模型并运用模型对数据进行分析与预测的一门学科。统计学习包括监督学习、非监督学习、半监督学习和强化学习。

2．统计学习方法三要素——模型、策略、算法，对理解统计学习方法起到提纲挈领的作用。

3．本书主要讨论监督学习，监督学习可以概括如下：从给定有限的训练数据出发， 假设数据是独立同分布的，而且假设模型属于某个假设空间，应用某一评价准则，从假设空间中选取一个最优的模型，使它对已给训练数据及未知测试数据在给定评价标准意义下有最准确的预测。

4．统计学习中，进行模型选择或者说提高学习的泛化能力是一个重要问题。如果只考虑减少训练误差，就可能产生过拟合现象。模型选择的方法有正则化与交叉验证。学习方法泛化能力的分析是统计学习理论研究的重要课题。

5．分类问题、标注问题和回归问题都是监督学习的重要问题。本书中介绍的统计学习方法包括感知机、$k$近邻法、朴素贝叶斯法、决策树、逻辑斯谛回归与最大熵模型、支持向量机、提升方法、EM算法、隐马尔可夫模型和条件随机场。这些方法是主要的分类、标注以及回归方法。它们又可以归类为生成方法与判别方法。
## 笔记

* 统计学习或机器学习一般包括监督学习、无监督学习、强化学习，有时还包括半监督学习、主动学习
## 监督学习
* 监督学习指从标注数据中学习预测模型的机器学习问题，其本质是学习输入到输出的映射的统计规律。
* 输入变量$X$和输出变量$Y$有不同的类型，可以是连续或是离散的。根据输入输出变量的不同类型，对预测任务给予不同的名称：输入与输出均为连续变量的预测问题称为**回归问题**；输出变量为有限个离散变量的预测问题称为**分类问题**；输入与输出变量均为变量序列的预测问题称为**标注问题**。
## 无监督学习
* 无监督学习指从无标注数据中学习预测模型的机器学习问题，其本质是学习数据中的统计规律或内在结构。
* 无监督学习旨在从假设空间中选出在给定评价标准下的最优模型，模型可以实现对数据的聚类、降维或是概率估计。
## 强化学习
* 强化学习指智能系统在与环境的连续互动中学习最优行为策略的机器学习问题，其本质是学习最优的序贯决策。
*  智能系统的目标不是短期奖励的最大化，而是长期累积奖励的最大化。强化学习过程中，系统不断地试错，以达到学习最优策略地目的。

## 半监督学习与主动学习
* 半监督学习指利用标注数据和未标注数据学习预测模型地机器学习问题。其旨在利用未标注数据中的信息，辅助标注数据，进行监督学习，以较低的成本达到较好的学习效果。
* 主动学习是指机器不断主动给出实例让教师进行标注，然后利用标注数据学习预测模型的机器学习问题。主动学习的目标是找出对学习最有帮助的实例让教师标注，以较小的标注代价达到较好的学习效果。
* 这两种学习更接近监督学习。

## 实现统计学习方法的步骤


1. 得到一个有限的训练数据集合
2. 确定包含所有可能的模型的**假设空间**，即学习**模型的集合**
3. 确定模型选择的准则，即学习的**策略**
 1. 实现求解最优模型的算法，即学习的**算法**
4. 通过学习方法选择最优的模型
5. 利用学习的最优模型对新数据进行预测或分析

* 在上述步骤中涵盖了统计学习方法三要素：模型，策略，算法


* 在监督学习过程中，模型就是所要学习的**条件概率分布**或者**决策函数**。
注意书中的这部分描述，整理了一下到表格里：

|              | 假设空间$\mathcal F$                                         | 输入空间$\mathcal X$ | 输出空间$\mathcal Y$ | 参数空间      |
| ------------ | ------------------------------------------------------------ | -------------------- | -------------------- | ------------- |
| 决策函数     | $\mathcal F =\{f$\|$Y=f_{\theta}(x), \theta \in \bf R \it ^n\}$ | 变量                 | 变量                 | $\bf R\it ^n$ |
| 条件概率分布 | $\mathcal F =\{P$\|$P_\theta(Y$\|$X), \theta \in \bf R \it ^n\}$ | 随机变量             | 随机变量             | $\bf R\it ^n$ |

## 损失函数与风险函数

*  **损失函数**度量模型**一次预测**的好坏，**风险函数**度量**平均意义**下模型预测的好坏。

* 损失函数(loss function)或代价函数(cost function)定义为给定输入$X$的**预测值$f(X)$**和**真实值$Y$**之间的**非负实值**函数，记作$L(Y,f(X))$。

* 风险函数(risk function)或期望损失(expected loss)和模型的泛化误差的形式是一样的
$R_{exp}(f)=E_p[L(Y, f(X))]=\int_{\mathcal X\times\mathcal Y}L(y,f(x))P(x,y)\, {\rm d}x{\rm d}y$         
上式是模型$f(X)$关于联合分布$P(X,Y)$的**平均意义下的**损失(期望损失)，但是因为$P(X,Y)$是未知的，所以前面的用词是**期望**，以及**平均意义下的**。这个表示其实就是损失的均值，反映了对整个数据的预测效果的好坏。

* **经验风险**(empirical risk)或**经验损失**(empirical loss)
   $R_{emp}(f)=\frac{1}{N}\sum^{N}_{i=1}L(y_i,f(x_i))$
   上式是模型$f$关于**训练样本集**的平均损失。根据大数定律，当样本容量N趋于无穷大时，经验风险趋于期望风险。

* **结构风险**(structural risk)
   $R_{srm}(f)=\frac{1}{N}\sum_{i=1}^{N}L(y_i,f(x_i))+\lambda J(f)$
   其中$J(f)$为模型复杂度, $\lambda \geqslant 0$是系数，用以权衡经验风险和模型复杂度。

## 常用损失函数

损失函数数值越小，模型就越好

* 0-1损失
   $L(Y,f(X))=\begin{cases}1, Y \neq f(X) \\0, Y=f(X) \end{cases}$
* 平方损失
   $L(Y,f(X))=(Y-f(X))^2$
* 绝对损失
   $L(Y,f(X))=|Y-f(X)|$

* 对数损失
$L(Y,P(Y|X))=−logP(Y|X)$



## ERM与SRM

经验风险最小化(ERM)与结构风险最小化(SRM)

* **极大似然估计**是经验风险最小化的一个例子。当模型是条件概率分布，损失函数是对数损失函数时，经验风险最小化等价于极大似然估计，下面习题1.2中给出了证明。
* 结构风险最小化等价于**正则化**
* **贝叶斯估计**中的**最大后验概率估计**是结构风险最小化的一个例子。当模型是条件概率分布，损失函数是对数损失函数，**模型复杂度由模型的先验概率表示**时，结构风险最小化等价于最大后验概率估计。

## 算法
* 算法是指学习模型的具体计算方法。统计学习基于训练数据集，根据学习策略，从假设空间中选择最优模型，最后需要考虑用什么杨的计算方法来求解最优模型。

## 模型评估与选择
* 训练误差和测试误差是模型关于数据集的平均损失。

* 注意：统计学习方法具体采用的损失函数未必是评估时使用的损失函数。
* 过拟合是指学习时选择的模型所包含的参数过多，以至出现这一模型对已知数据预测得很好，但对未知数据预测得很差的现象。可以说模型选择旨在避免过拟合并提高模型的预测能力。

## 正则化与交叉验证
* 模型选择的典型方法是**正则化**.。正则化项一般是模型复杂度的单调递增函数，模型越复杂，正则化就越大。比如，正则化项可以是模型参数向量的范数。
* $L(w)=\frac{1}{N}\sum_{i=1}^{N}(f(x_i;w)-y_i)^2+\frac{\lambda}{2}\|w\|^2$
* $\|w\|$表示向量$w$的$L_2$范数
* 正则化符合奥卡姆剃刀原理：如无必要，勿增实体。在应用于模型选择中时，可以理解为：在所有可能选择的模型中，能够很好地解释已知数据并且十分简单的才是最好的模型，也是应该选择的模型。
* 交叉验证的基本想法时重复地利用数据；把给定地数据进行切分，将切分的数据集组合为训练集和测试集，在此基础上反复地进行训练、测试以及模型选择。
* 主要有简单交叉验证，S折交叉验证，留一交叉验证三种。
* 在算法学习的过程中，测试集可能是固定的，但是验证集和训练集可能是变化的。比如S折交叉验证的情况下，分成S折之后，其中的S-1折作为训练集，1折作为验证集，计算这S个模型每个模型的平均测试误差，最后选择平均测试误差最小的模型。这个过程中用来验证模型效果的那一折数据就是验证集。

## 生成模型与判别模型

**监督学习方法**可分为**生成方法**(generative approach)与**判别方法**(discriminative approach)

* **生成方法(generative approach)**

  - 可以还原出**联合概率分布**$P(X,Y)$
  - 收敛速度快, 当样本容量增加时, 学到的模型可以更快收敛到真实模型
  - 当存在隐变量时仍可以用

* **判别方法(discriminative approach)**

   - 直接学习**条件概率**$P(Y|X)$或者**决策函数**$f(X)$
   - 直接面对预测, 往往学习准确率更高
  - 可以对数据进行各种程度的抽象,  定义特征并使用特征, 可以简化学习问题


### 使用最小二乘法拟和曲线

高斯于1823年在误差$e_1,…,e_n$独立同分布的假定下,证明了最小二乘方法的一个最优性质: 在所有无偏的线性估计类中,最小二乘方法是其中方差最小的！
对于数据$(x_i, y_i)   (i=1, 2, 3...,m)$

拟合出函数$h(x)$

有误差，即残差：$r_i=h(x_i)-y_i$

此时$L2$范数(残差平方和)最小时，$h(x)$ 和 $y$ 相似度最高，更拟合

一般的$H(x)$为$n$次的多项式，$H(x)=w_0+w_1x+w_2x^2+...w_nx^n$

$w(w_0,w_1,w_2,...,w_n)$为参数

最小二乘法就是要找到一组 $w(w_0,w_1,w_2,...,w_n)$ ，使得$\sum_{i=1}^n(h(x_i)-y_i)^2$ (残差平方和) 最小

即，求 $min\sum_{i=1}^n(h(x_i)-y_i)^2$

----

举例：我们用目标函数$y=sin2{\pi}x$, 加上一个正态分布的噪音干扰，用多项式去拟合【例1.1】

```python
import numpy as np
import scipy as sp
from scipy.optimize import leastsq
import matplotlib.pyplot as plt
%matplotlib inline
```
* ps: numpy.poly1d([1,2,3])  生成  $1x^2+2x^1+3x^0$*

```python
# 目标函数
def real_func(x):
    return np.sin(2*np.pi*x)

# 多项式
def fit_func(p, x):
    f = np.poly1d(p)
    return f(x)

# 残差
def residuals_func(p, x, y):
    ret = fit_func(p, x) - y
    return ret
```

```python
# 十个点
x = np.linspace(0, 1, 10)
x_points = np.linspace(0, 1, 1000)
# 加上正态分布噪音的目标函数的值
y_ = real_func(x)
y = [np.random.normal(0, 0.1) + y1 for y1 in y_]


def fitting(M=0):
    """
    M    为 多项式的次数
    """
    # 随机初始化多项式参数
    p_init = np.random.rand(M + 1)
    # 最小二乘法
    p_lsq = leastsq(residuals_func, p_init, args=(x, y))
    print('Fitting Parameters:', p_lsq[0])

    # 可视化
    plt.plot(x_points, real_func(x_points), label='real')
    plt.plot(x_points, fit_func(p_lsq[0], x_points), label='fitted curve')
    plt.plot(x, y, 'bo', label='noise')
    plt.legend()
    return p_lsq
```

### M=0

```python
# M=0
p_lsq_0 = fitting(M=0)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210421184910220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUwODI2NQ==,size_16,color_FFFFFF,t_70)
### M=1

```python
# M=1
p_lsq_1 = fitting(M=1)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210421184935659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUwODI2NQ==,size_16,color_FFFFFF,t_70)
### M=3 

```python
# M=3
p_lsq_3 = fitting(M=3)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021042118495862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUwODI2NQ==,size_16,color_FFFFFF,t_70)
### M=9

```python
# M=9
p_lsq_9 = fitting(M=9)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210421185018732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUwODI2NQ==,size_16,color_FFFFFF,t_70)
当M=9时，多项式曲线通过了每个数据点，但是造成了过拟合

### 正则化
结果显示过拟合， 引入正则化项(regularizer)，降低过拟合
$$
Q(x)=\sum_{i=1}^n(h(x_i)-y_i)^2+\lambda||w||^2
$$
回归问题中，损失函数是平方损失，正则化可以是参数向量的L2范数,也可以是L1范数。

- L1: regularization\*abs(p)

- L2: 0.5 \* regularization \* np.square(p)

```python
regularization = 0.0001

def residuals_func_regularization(p, x, y):
    ret = fit_func(p, x) - y
    ret = np.append(ret,
                    np.sqrt(0.5 * regularization * np.square(p)))  # L2范数作为正则化项
    return ret
```

```python
# 最小二乘法,加正则化项
p_init = np.random.rand(9 + 1)
p_lsq_regularization = leastsq(
    residuals_func_regularization, p_init, args=(x, y))
```

```python
plt.plot(x_points, real_func(x_points), label='real')
plt.plot(x_points, fit_func(p_lsq_9[0], x_points), label='fitted curve')
plt.plot(
    x_points,
    fit_func(p_lsq_regularization[0], x_points),
    label='regularization')
plt.plot(x, y, 'bo', label='noise')
plt.legend()
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210421185207166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUwODI2NQ==,size_16,color_FFFFFF,t_70)

# 第1章统计学习方法概论-习题

## 习题1.1
&emsp;&emsp;说明伯努利模型的极大似然估计以及贝叶斯估计中的统计学习方法三要素。伯努利模型是定义在取值为0与1的随机变量上的概率分布。假设观测到伯努利模型$n$次独立的数据生成结果，其中$k$次的结果为1，这时可以用极大似然估计或贝叶斯估计来估计结果为1的概率。

**解答：**

伯努利模型的极大似然估计以及贝叶斯估计中的**统计学习方法三要素**如下：  
 * 伯努利模型是定义在取值为0与1的随机变量上的概率分布。统计学分为两派：经典统计学派和贝叶斯统计学派。两者的不同主要是，经典统计学派认为模型已定，参数未知，参数是固定的，只是还不知道；贝叶斯统计学派是通过观察到的现象对概率分布中的主观认定不断进行修正。
* **极大似然估计**用的是经典统计学派的策略，**贝叶斯估计**用的是贝叶斯统计学派的策略；为了得到使经验风险最小的参数值，使用的算法都是对经验风险求导，使导数为0。
1. **极大似然估计**  
**模型：** $\mathcal{F}=\{f|f_p(x)=p^x(1-p)^{(1-x)}\}$  
**策略：** 最大化似然函数  
**算法：** $\displaystyle \mathop{\arg\min}_{p} L(p)= \mathop{\arg\min}_{p} \binom{n}{k}p^k(1-p)^{(n-k)}$  
2. **贝叶斯估计**  
**模型：** $\mathcal{F}=\{f|f_p(x)=p^x(1-p)^{(1-x)}\}$  
**策略：** 求参数期望  
**算法：**
$$\begin{aligned}  E_\pi\big[p \big| y_1,\cdots,y_n\big]
& = {\int_0^1}p\pi (p|y_1,\cdots,y_n) dp \\
& = {\int_0^1} p\frac{f_D(y_1,\cdots,y_n|p)\pi(p)}{\int_{\Omega}f_D(y_1,\cdots,y_n|p)\pi(p)dp}dp \\
& = {\int_0^1}\frac{p^{k+1}(1-p)^{(n-k)}}{\int_0^1 p^k(1-p)^{(n-k)}dp}dp
\end{aligned}$$

**伯努利模型的极大似然估计：**  
定义$P(Y=1)$概率为$p$，可得似然函数为：

$$L(p)=f_D(y_1,y_2,\cdots,y_n|\theta)=\binom{n}{k}p^k(1-p)^{(n-k)}$$

方程两边同时对$p$求导，则：

$$\begin{aligned}
0 & = \binom{n}{k}[kp^{k-1}(1-p)^{(n-k)}-(n-k)p^k(1-p)^{(n-k-1)}]\\
& = \binom{n}{k}[p^{(k-1)}(1-p)^{(n-k-1)}(k-np)]
\end{aligned}$$

可解出$p$的值为$p=0,p=1,p=k/n$，显然$\displaystyle P(Y=1)=p=\frac{k}{n}$  

**伯努利模型的贝叶斯估计：**  
定义$P(Y=1)$概率为$p$，$p$在$[0,1]$之间的取值是等概率的，因此先验概率密度函数$\pi(p) = 1$，可得似然函数为： 

$$L(p)=f_D(y_1,y_2,\cdots,y_n|\theta)=\binom{n}{k}p^k(1-p)^{(n-k)}$$  

根据似然函数和先验概率密度函数，可以求解$p$的条件概率密度函数：

$$\begin{aligned}\pi(p|y_1,\cdots,y_n)&=\frac{f_D(y_1,\cdots,y_n|p)\pi(p)}{\int_{\Omega}f_D(y_1,\cdots,y_n|p)\pi(p)dp}\\
&=\frac{p^k(1-p)^{(n-k)}}{\int_0^1p^k(1-p)^{(n-k)}dp}\\
&=\frac{p^k(1-p)^{(n-k)}}{B(k+1,n-k+1)}
\end{aligned}$$

所以$p$的期望为：

$$\begin{aligned}
E_\pi[p|y_1,\cdots,y_n]&={\int}p\pi(p|y_1,\cdots,y_n)dp \\
& = {\int_0^1}\frac{p^{(k+1)}(1-p)^{(n-k)}}{B(k+1,n-k+1)}dp \\
& = \frac{B(k+2,n-k+1)}{B(k+1,n-k+1)}\\
& = \frac{k+1}{n+2}
\end{aligned}$$

$\therefore \displaystyle P(Y=1)=\frac{k+1}{n+2}$

 * 统计学习方法的三要素为模型，策略，算法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/202104211923513.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUwODI2NQ==,size_16,color_FFFFFF,t_70)


## 习题1.2
&emsp;&emsp;通过经验风险最小化推导极大似然估计。证明模型是条件概率分布，当损失函数是对数损失函数时，经验风险最小化等价于极大似然估计。

**解答：**

  * 模型是条件概率分布：$P_θ(Y|X)$，
损失函数是对数损失函数：$L(Y,P_θ(Y|X))=−logP_θ(Y|X)$ 
经验风险为：
  * $$ \begin{aligned}
R_{emp}(f)&=\frac{1}{N}\sum_{i=1}^{N}L(y_i,f(x_i)) \\
&=\frac{1}{N}\sum_{i=1}^{N}-logP(y_i|x_i) \\
&=-\frac{1}{N}\sum_{i=1}^{N}logP(y_i|x_i)
\end{aligned}$$


   * 极大似然估计的似然函数为： 
    $$L(\theta)=\prod_DP_{\theta}(Y|X)$$，取对数
    
    $$log(L(\theta))=\sum_DlogP_{\theta}(Y|X)$$

$$arg\max_\theta\sum_DlogP_{\theta}(Y|X)=arg\min_{\theta}\sum_D-logP_{\theta}(Y|X)$$

* 因此，当损失函数是对数损失函数时，经验风险最小化等价于极大似然估计。