Data Science(part V)
========================================================

# Regression Models

## 导论

- Francis Galton 1885年用父母身高预测子女身高的案例
- 考虑单变量的数据代表：最小二乘值
  - 最小二乘值物理意义为质心
  - 最小二乘统计学意义是平均值
  - 可用不等式解 也可用求导方法解

$$ 
\begin{align} 
\sum_{i=1}^n (Y_i - \mu)^2 & = \
\sum_{i=1}^n (Y_i - \bar Y + \bar Y - \mu)^2 \\ 
& = \sum_{i=1}^n (Y_i - \bar Y)^2 + \
2 \sum_{i=1}^n (Y_i - \bar Y)  (\bar Y - \mu) +\
\sum_{i=1}^n (\bar Y - \mu)^2 \\
& = \sum_{i=1}^n (Y_i - \bar Y)^2 + \
2 (\bar Y - \mu) \sum_{i=1}^n (Y_i - \bar Y)  +\
\sum_{i=1}^n (\bar Y - \mu)^2 \\
& = \sum_{i=1}^n (Y_i - \bar Y)^2 + \
2 (\bar Y - \mu)  (\sum_{i=1}^n Y_i - n \bar Y) +\
\sum_{i=1}^n (\bar Y - \mu)^2 \\
& = \sum_{i=1}^n (Y_i - \bar Y)^2 + \sum_{i=1}^n (\bar Y - \mu)^2\\ 
& \geq \sum_{i=1}^n (Y_i - \bar Y)^2 \
\end{align} 
$$

- 通过原点的回归
  - 最小化$\sum_{i=1}^n (Y_i - X_i \beta)^2$
  - 两变量关系用回归线解释
  
## 术语

- $X_1, X_2, \ldots, X_n$ 表示 $n$ 个数据点
- $Y_1, \ldots , Y_n$ 表示另外 $n$ 个数据点
- 用希腊字母表示不知道的东西 如 $\mu$
- 大写字母表示概念值 小写字母表示真实值 如 $P(X_i > x)$
- $\bar X = \frac{1}{n}\sum_{i=1}^n X_i$ 表示均值 数据的中心趋向
- $\tilde X_i = X_i - \bar X$ 表示对数据中心化 均值为0
- 均值为数据的最小二乘估计
- $S^2 = \frac{1}{n-1} \sum_{i=1}^n (X_i - \bar X)^2 
= \frac{1}{n-1} \left( \sum_{i=1}^n X_i^2 - n \bar X ^ 2 \right)$ 表示方差
- $S$ 为标准差 数据的离散程度
- $X_i / s$ 表示数据缩放  方差为1
- $Z_i = \frac{X_i - \bar X}{s}$ 表示数据的标准化 先中心化再标准化
- $Cov(X, Y) = \frac{1}{n-1}\sum_{i=1}^n (X_i - \bar X) (Y_i - \bar Y)= \frac{1}{n-1}\left( \sum_{i=1}^n X_i Y_i - n \bar X \bar Y\right)$ 表示协方差
- $Cor(X, Y) = \frac{Cov(X, Y)}{S_x S_y}$ 表示相关性
  - $Cor(X, Y) = Cor(Y, X)$
  - $-1 \leq Cor(X, Y) \leq 1$
  - $Cor(X, Y)$ 度量线性关系强度
  - $Cor(X, Y) = 0$ 表示无线性关系

## 回归线的最小二乘回归

- 用最小二乘法寻找回归线 最小化 $\sum_{i=1}^n \{Y_i - (\beta_0 + \beta_1 X_i)\}^2$
- 如果定义 $\mu_i = \beta_0$ $\hat \beta_0 = \bar Y$ 不考虑其他变量 $Y$ 的均值就是最小二乘估计
- 如果定义 $\mu_i = X_i \beta_1$ $\hat \beta_1 = \frac{\sum_{i=1^n} Y_i X_i}{\sum_{i=1}^n X_i^2}$ 如果考虑过原点线的回归 斜率如上
- 如果考虑 $\mu_i = \beta_0 + \beta_1 X_i$ 

$$\begin{align} \
\sum_{i=1}^n (Y_i - \hat \mu_i) (\hat \mu_i - \mu_i) 
= & \sum_{i=1}^n (Y_i - \hat\beta_0 - \hat\beta_1 X_i) (\hat \beta_0 + \hat \beta_1 X_i - \beta_0 - \beta_1 X_i) \\
= & (\hat \beta_0 - \beta_0) \sum_{i=1}^n (Y_i - \hat\beta_0 - \hat \beta_1 X_i) + (\beta_1 - \beta_1)\sum_{i=1}^n (Y_i - \hat\beta_0 - \hat \beta_1 X_i)X_i\\
\end{align} $$

- 解为$\hat \beta_1 = Cor(Y, X) \frac{Sd(Y)}{Sd(X)} ~~~ \hat \beta_0 = \bar Y - \hat \beta_1 \bar X$
- 如果标准化数据 $\{ \frac{X_i - \bar X}{Sd(X)}, \frac{Y_i - \bar Y}{Sd(Y)}\}$ 解为$Cor(Y, X)$
- 回归是因变量向自己均值回归与向自变量相关回归的平衡

## 统计线性回归模型

- 最小二乘是一种估计方法，做推断需要模型
- 建立线性回归的概率模型$Y_i = \beta_0 + \beta_1 X_i + \epsilon_{i}$
- $\epsilon_{i}$ 为 iid $N(0, \sigma^2)$
- $E[Y_i ~|~ X_i = x_i] = \mu_i = \beta_0 + \beta_1 x_i$
- $Var(Y_i ~|~ X_i = x_i) = \sigma^2$
- 对$N(\mu_i, \sigma^2)$独立变量 $Y$ 进行极大似然估计
- ${\cal L}(\beta, \sigma) = \prod_{i=1}^n \left\{(2 \pi \sigma^2)^{-1/2}\exp\left(-\frac{1}{2\sigma^2}(y_i - \mu_i)^2 \right) \right\}$
- 取对数有 $-2 \log\{ {\cal L}(\beta, \sigma) \} = \frac{1}{\sigma^2} \sum_{i=1}^n (y_i - \mu_i)^2 + n\log(\sigma^2)$
- 最小二乘估计就是极大似然估计
- $\hat \beta_1 = Cor(Y, X) \frac{Sd(Y)}{Sd(X)} ~~~ \hat \beta_0 = \bar Y - \hat \beta_1 \bar X$
- 截距是自变量为0时 $Y$ 的期望 斜率是自变量变化一个单位对 $Y$ 的影响

## 残差

- 模型 $Y_i = \beta_0 + \beta_1 X_i + \epsilon_i$ 
- 预测值 $\hat Y_i = \hat \beta_0 + \hat \beta_1 X_i$
- $e_i = Y_i - \hat Y_i$ 观察数据与回归线的垂直距离
- 最小二乘估计最小化残差 $\sum_{i=1}^n e_i^2$
- 残差 $e_i$ 可看作 $\epsilon_i$ 的估计
- 可证 $E[e_i] = 0$ 模型中考虑截距 $\sum_{i=1}^n e_i = 0$ 考虑自变量 $\sum_{i=1}^n e_i X_i = 0$
- 残差可用来评价模型效果
- 残差波动不同于模型波动
- 残差波动 $\sigma^2$ 的极大似然估计为 $\frac{1}{n}\sum_{i=1}^n e_i^2$
- $\hat \sigma^2 = \frac{1}{n-2}\sum_{i=1}^n e_i^2$ 为无偏估计

$$
\begin{align}
\sum_{i=1}^n (Y_i - \bar Y)^2 
& = \sum_{i=1}^n (Y_i - \hat Y_i + \hat Y_i - \bar Y)^2 \\
& = \sum_{i=1}^n (Y_i - \hat Y_i)^2 + 
2 \sum_{i=1}^n  (Y_i - \hat Y_i)(\hat Y_i - \bar Y) + 
\sum_{i=1}^n  (\hat Y_i - \bar Y)^2 \\
\end{align}
$$

- 其中 $(Y_i - \hat Y_i) = \{Y_i - (\bar Y - \hat \beta_1 \bar X) - \hat \beta_1 X_i\} = (Y_i - \bar Y) - \hat \beta_1 (X_i - \bar X)$
- $(\hat Y_i - \bar Y) = (\bar Y - \hat \beta_1 \bar X - \hat \beta_1 X_i - \bar Y )= \hat \beta_1  (X_i - \bar X)$
- 有$\sum_{i=1}^n  (Y_i - \hat Y_i)(\hat Y_i - \bar Y) = \sum_{i=1}^n  \{(Y_i - \bar Y) - \hat \beta_1 (X_i - \bar X))\}\{\hat \beta_1  (X_i - \bar X)\}=\hat \beta_1 \sum_{i=1}^n (Y_i - \bar Y)(X_i - \bar X) -\hat\beta_1^2\sum_{i=1}^n (X_i - \bar X)^2= \hat \beta_1^2 \sum_{i=1}^n (X_i - \bar X)^2-\hat\beta_1^2\sum_{i=1}^n (X_i - \bar X)^2 = 0$
- 综上 $\sum_{i=1}^n (Y_i - \bar Y)^2 = \sum_{i=1}^n (Y_i - \hat Y_i)^2 + \sum_{i=1}^n  (\hat Y_i - \bar Y)^2$
- 有 Total Variation = Residual Variation + Regression Variation
- 模型解释部分$R^2 = \frac{\sum_{i=1}^n  (\hat Y_i - \bar Y)^2}{\sum_{i=1}^n (Y_i - \bar Y)^2} = 1 - \frac{\sum_{i=1}^n  (Y_i - \hat Y_i)^2}{\sum_{i=1}^n (Y_i - \bar Y)^2}$
- 已知 $(\hat Y_i - \bar Y) = \hat \beta_1  (X_i - \bar X)$ $\hat \beta_1 = Cor(Y, X)\frac{Sd(Y)}{Sd(X)}$ 有 $R^2 = \frac{\sum_{i=1}^n  (\hat Y_i - \bar Y)^2}{\sum_{i=1}^n (Y_i - \bar Y)^2}= \hat \beta_1^2  \frac{\sum_{i=1}^n(X_i - \bar X)^2}{\sum_{i=1}^n (Y_i - \bar Y)^2}= Cor(Y, X)^2$
- $R^2$ 实际上是相关性 $r$ 的平方 <- 线性模型的可解释性
- $R^2$ 会伴随样本数增加而增加 会因删除异常值而增加
- `data(anscombe);example(anscombe)`

## 回归推断

- $\frac{\hat \theta - \theta}{\hat \sigma_{\hat \theta}}$ 总符合正态分布或$t$分布
- 假设检验 $H_0 : \theta = \theta_0$ 与 $H_a : \theta >, <, \neq \theta_0$
- 置信区间 $\theta$ 通过 $\hat \theta \pm Q_{1-\alpha/2} \hat \sigma_{\hat \theta}$ 构建

$$
\begin{align}
Var(\hat \beta_1) & =
Var\left(\frac{\sum_{i=1}^n (Y_i - \bar Y) (X_i - \bar X)}{\sum_{i=1}^n (X_i - \bar X)^2}\right) \\
& = \frac{Var\left(\sum_{i=1}^n Y_i (X_i - \bar X) \right) }{\left(\sum_{i=1}^n (X_i - \bar X)^2 \right)^2} \\
& = \frac{\sum_{i=1}^n \sigma^2(X_i - \bar X)^2}{\left(\sum_{i=1}^n (X_i - \bar X)^2 \right)^2} \\
& = \frac{\sigma^2}{\sum_{i=1}^n (X_i - \bar X)^2} \\
\end{align}
$$

- $\sigma_{\hat \beta_1}^2 = Var(\hat \beta_1) = \sigma^2 / \sum_{i=1}^n (X_i - \bar X)^2$
- $\sigma_{\hat \beta_0}^2 = Var(\hat \beta_0)  = \left(\frac{1}{n} + \frac{\bar X^2}{\sum_{i=1}^n (X_i - \bar X)^2 }\right)\sigma^2$
- 这样 $\frac{\hat \beta_j - \beta_j}{\hat \sigma_{\hat \beta_j}}$ 遵守自由度为$n-2$的$t$分布或正态分布
- 在$x_0$ 回归线的标准误 $\hat \sigma\sqrt{\frac{1}{n} +  \frac{(x_0 - \bar X)^2}{\sum_{i=1}^n (X_i - \bar X)^2}}$
- 在$x_0$ 预测值的标准误 $\hat \sigma\sqrt{1 + \frac{1}{n} + \frac{(x_0 - \bar X)^2}{\sum_{i=1}^n (X_i - \bar X)^2}}$
- CI代表回归线在特定$x$处的变动 PI代表预测值在此处的变动 前者在回归线固定时不变 后者还要考虑预测值围绕回归线的变动 

> The prediction interval is the range in which future observation can be thought most likely to occur, whereas the confidence interval is where the mean of future observation is most likely to reside. From [here](http://stackoverflow.com/questions/9406139/r-programming-predict-prediction-vs-confidence/9406534#9406534)

## 多元回归

- 线性模型 $Y_i =  \beta_1 X_{1i} + \beta_2 X_{2i} + \ldots + \beta_{p} X_{pi} + \epsilon_{i} = \sum_{k=1}^p X_{ik} \beta_j + \epsilon_{i}$
- 最小化 $\sum_{i=1}^n \left(Y_i - \sum_{k=1}^p X_{ki} \beta_j\right)^2$ 最小二乘估计也是误差正态化的极大似然估计
- 最小二乘估计等价于 $\sum_{i=1}^n (Y_i - X_{1i}\hat \beta_1 - \ldots - X_{ip}\hat \beta_p) X_k = 0$ 本质上使其他参数固定解出一个 然后逐级代入 最后全部解出参数值 参考线性代数
- 参数代表固定其他参数后变动一个单位引发的变化
- 方差估计 $\hat \sigma^2 = \frac{1}{n-p} \sum_{i=1}^n e_i ^2$
- 参数标准误$\hat \sigma_{\hat \beta_k}$ $\frac{\hat \beta_k - \beta_k}{\hat \sigma_{\hat \beta_k}}$ 符合自由度 $n-p$ 的 $T$ 分布
- 多元模型中加入变量会导致原有变量的参数估计发生变化 甚至方向相反 一般是由于加入变量与原有变量存在共相关 导致两者参数估计都不准

```
n <- 100; x2 <- 1 : n; x1 <- .01 * x2 + runif(n, -.1, .1); y = -x1 + x2 + rnorm(n, sd = .01)
summary(lm(y ~ x1))$coef
summary(lm(y ~ x1 + x2))$coef
```

- `R` 会自动检测并消除变量生成的变量 如上面 `x2` 中需要加入 `runif(n,-.1,.1)` 才能得到结果
- 多元模型中包括分类变量考虑加入虚拟变量 $Y_i = \beta_0 + X_{i1} \beta_1 + \epsilon_{i}$ 属于该分类时 $E[Y_i] = \beta_0 + \beta_1$ 否则为$E[Y_i] = \beta_0$
- 分类变量截距有意义 代表其中一个分类 等同于其他分类与该分类进行 `t` 检验 如果模型中去掉截距 等同于所有分类与零进行 `t` 检验 参数系数为均值差 可用 `relevel(data,'name')` 来指定比对对象
- 两变量均值差的标准误通过 $Var(\hat \beta_B - \hat \beta_C) = Var(\hat \beta_B) + Var(\hat \beta_C) - 2 Cov(\hat \beta_B, \hat \beta_C)$ 来计算进行推断
- 交互作用 $E[Y_i | X_{1i}=x_1, X_{2i}=x_2] = \beta_0 + \beta_1 x_{1} + \beta_2 x_{2} + \beta_3 x_{1}x_{2}$ 中交互作用参数实际表示 $E[Y_i | X_{1i}=x_1+1, X_{2i}=x_2+1]-E[Y_i | X_{1i}=x_1, X_{2i}=x_2+1]-E[Y_i | X_{1i}=x_1+1, X_{2i}=x_2]-E[Y_i | X_{1i}=x_1, X_{2i}=x_2] =\beta_3$ 各交互参数变化一单位响应变化
- 多元回归的参数解释需要考虑清楚变量类型与交互作用
- 多元回归中变量与响应 变量与变量间的相关性要全盘考虑 通过模拟观察决定

## 模型诊断与选择

- 通过残差诊断 最小二乘决定均值为零 方差通过 $\hat \sigma^2 = \frac{\sum_{i=1}^n e_i^2}{n-p}$ 进行无偏估计
- 异常值判断 对回归关系包括系数与其标准误的影响 残差的分布检验等 `?influence.measures`

> There are known knowns. These are things we know that we know. There are known unknowns. That is to say, there are things that we know we don't know. But there are also unknown unknowns. There are things we don't know we don't know. Donald Rumsfeld

- 随机化有助于平衡未知变量
- 参数方差膨胀 共相关或随机相关 `vif`来检验 协变量在欠拟合下有偏
- 协变量的选择需要专业知识与经验

## 广义线性模型

- Nelder 与 Wedderburn 1972年提出
- 响应是指数家族模型 模型组成部分是线性的 线性预测变量与响应通过连接函数联系
- 线性模型
  - $Y_i \sim N(\mu_i, \sigma^2)$
  - $\eta_i = \sum_{k=1}^p X_{ik} \beta_k$
  - $g(\mu) = \eta$
  - 似然模型为 $Y_i = \sum_{k=1}^p X_{ik} \beta_k + \epsilon_{i}$ $\epsilon_i \stackrel{iid}{\sim} N(0, \sigma^2)$
- logistic 模型
  - $Y_i \sim Bernoulli(\mu_i)$
  - $\eta_i = \sum_{k=1}^p X_{ik} \beta_k$
  - $g(\mu) = \eta = \log\left( \frac{\mu}{1 - \mu}\right)$ $g$为logit函数
  - 似然函数为 $\prod_{i=1}^n \mu_i^{y_i} (1 - \mu_i)^{1-y_i} = \exp\left(\sum_{i=1}^n y_i \eta_i \right) \prod_{i=1}^n (1 + \eta_i)^{-1}$
- 泊松模型
  - $Y_i \sim Poisson(\mu_i)$
  - $\eta_i = \sum_{k=1}^p X_{ik} \beta_k$
  - $g(\mu) = \eta = \log(\mu)$
  - 似然函数为 $\prod_{i=1}^n (y_i !)^{-1} \mu_i^{y_i}e^{-\mu_i}\propto \exp\left(\sum_{i=1}^n y_i \eta_i - \sum_{i=1}^n \mu_i\right)$
- 似然函数与数据的联系 $\sum_{i=1}^n y_i \eta_i = \sum_{i=1}^n y_i\sum_{k=1}^p X_{ik} \beta_k = \sum_{k=1}^p \beta_k\sum_{i=1}^n X_{ik} y_i$ 只有$\sum_{i=1}^n X_{ik} y_i$
- 极大似然估计的解 $0=\sum_{i=1}^n \frac{(Y_i - \mu_i)}{Var(Y_i)}W_i$ $W_i$是连接函数的反函数的微分
- 响应的方差中线性模型 $Var(Y_i) = \sigma^2$ 是常数 logistic 模型 $Var(Y_i) = \mu_i (1 - \mu_i)$ 泊松模型 $Var(Y_i) = \mu_i$
- 可通过对模型方差增加调谐参数 $\phi$ 使模型更灵活 quasi-likelihood
- 模型求解为 $\hat \beta_k$ 及可能的 $\hat \phi$
- 线性预测变量关系 $\hat \eta = \sum_{k=1}^p X_k \hat \beta_k$
- 平均响应 $\hat \mu = g^{-1}(\hat \eta)$
- 系数解释 $g(E[Y | X_k = x_k + 1, X_{\sim k} = x_{\sim k}]) - g(E[Y | X_k = x_k, X_{\sim k}=x_{\sim k}]) = \beta_k$

## 二元响应

- $\log\left(\frac{\rm{Pr}(RW_i | RS_i, b_0, b_1 )}{1-\rm{Pr}(RW_i | RS_i, b_0, b_1)}\right) = b_0 + b_1 RS_i$
- $b_0$ 预测变量为零时胜率对数
- $b_1$ 预测变量变化一个单位胜率的改变对数
- $\exp(b_1)$ 预测变量变化一个单位胜率的改变

## 计数或速率响应

- $\log\left(E[NH_i | JD_i, b_0, b_1]\right) = b_0 + b_1 JD_i$
- $e^{E[\log(Y)]}$ $Y$ 的几何平均值
- $e^{\beta_0}$ 第零天的几何平均值
- $e^{\beta_1}$ 每天相对增加或减少的几何平均值
- 通过设置 `offset` 可用来估计增长率
- 注意方差膨胀与[零膨胀](http://cran.r-project.org/web/packages/pscl/index.html)问题

## 分段平滑

- 可用线性回归拟合曲线 原理是分段拟合连接
- 断点平滑可用二次项
- 分段项可看作基进行组合

# Practical Machine Learning

- 问题 -> 数据 -> 特征 -> 算法 -> 参数 -> 评价

> The combination of some data and an aching desire for an answer does not ensure that a reasonable answer can be extracted from a given body of data. John Tukey

- 数据质量优先于模型
- 不要自动特征选择
- 算法的可扩展性与计算性能要考虑
- 数据过拟合问题 数据总是由信号与噪音组成 但会被算法无差别对待
- 数据要与问题相关 低相关度的组合可能产生高相关度

## 研究设计

- 定义错误率
- 将数据分割为训练集 预测集 验证集
- 在训练集上使用交叉检验选择特征与预测算法
- 在预测集或验证集上使用一次数据
- 预测效果起码要优于瞎猜
- 避免使用小样本
- 比例为 60% 训练集 20% 预测集 20% 验证集 或 60% 训练集 40% 预测集 或小样本交叉检验
- 注意数据结构 时序分析要对数据分段采样

## 错误率

- 真阳性 真的是对的 TP
- 假阳性 真的是错的 FP Type I
- 真阴性 假的是错的 TN
- 假阴性 假的是对的 FN Type II
- 灵敏度 TP/(TP+FP)
- 特异性 TN/(TN+FN)
- 均方差 MSE $\frac{1}{n} \sum_{i=1}^n (Prediction_i - Truth_i)^2$
- 均方误 RMSE $\sqrt{\frac{1}{n} \sum_{i=1}^n(Prediction_i - Truth_i)^2}$
- 中位差 Median absolute deviation
- 准确性 (TP+TN)/(TP+FP+TN+FP)
- 一致性 [kappa值](https://en.wikipedia.org/wiki/Cohen%27s_kappa)

## ROC 曲线

- 分类问题寻找判别阈值 满足一定TP下最小FP的模型
- FP v.s.TP 作图
- AUC 曲线下面积表示选择标准 一般超过80%
- 对角线是随机猜的结果

## 交叉检验

- 训练集上的操作
- 训练集上再分为训练集与测试集
- 在测试集上评价 重复并平均化测试集错误
- 用来进行变量 模型 参数选择
- 随机 分组 留一
- 分组多方差大 分组少有偏差
- 有放回的为bootstrap 不建议用

## `caret` 包

- 数据清洗 预处理
- 数据分割 `createDataPartition` 数据比例 重采样 产生时间片段
- 训练检验整合函数 `train` `predict` 
- 模型对比 
- 算法整合为选项 线性判别 回归 朴素贝叶斯 支持向量机 分类与回归树 随机森林 Boosting 等

## 数据分割

- `train <- createDataPartition(y=spam$type,p=0.75, list=FALSE)` 数据三一分 得到index
- `folds <- createFolds(y=spam$type,k=10,list=TRUE,returnTrain=TRUE)` 数据分10份 返回每一份列表
- `folds <- createResample(y=spam$type,times=10,list=TRUE)` 数据bootstrap重采样 返回每一份列表
- `folds <- createTimeSlices(y=tme,initialWindow=20,horizon=10)` 时序数据重采样 产生20为窗口时序片段的训练集与预测集

## 训练选项

- `args(train.default)` 通过 `method` 控制算法 `metric` 控制算法评价 `trainControl` 控制训练方法
- `trainControl`中 `method`选择模型选择方法 如bootstrap 交叉检验 留一法 `number` 控制次数 `repeats` 控制重采样次数 `seed` 控制可重复性 总体设置一个 具体每一次用列表设置控制具体过程 特别是并行模型

## 预测变量作图

- `featurePlot` 
- `ggplot2`

## 数据预处理

- `train` 中的 `preProcess=c("center","scale")` 标准化
- `spatialSign` 该转化可提高计算效率 有偏
- `preProcess(training[,-58],method=c("BoxCox"))` 正态化转化
- `method="knnImpute"` 用最小邻近法填补缺失值
- `nearZeroVar` 去除零方差变量
- `findCorrelation` 去除相关变量
- `findLinearCombos` 去除线性组合变量
- `classDist` 测定分类变量的距离 生成新变量
- 测试集也要预处理

## 协变量生成

- 原始数据提取特征
- 提取特征后生成新变量
- 因子变量要转为虚拟变量
- 样条基变量 `splines` 包中的 `bs`
- 数据压缩 `preProcess` 中 `method` 设置为 `pca` `pcaComp` 指定主成分个数

## 线性回归&多元线性回归

- $ED_i = b_0 + b_1 WT_i + e_i$ 基本模型
- 参见前面回归部分

## 树

- 迭代分割变量
- 在最大化预测时分割
- 评估分支的同质性
- 多个树的预测更好
  - 优点 容易解释应用 可用在神经网络上
  - 缺点 不容易交叉验证 不确定性不宜估计 结果可能变化
-算法
  - 先在一个组里用所有的变量计算
  - 寻找最容易分离结果的变量
  - 把数据按照该变量节点分为两组
  - 在每一个组中寻找最好的分离变量
  - 迭代直到过程结束    
  - 节点纯度用 Gini 系数或 交叉墒来衡量
- `rattle` 包的 `fancyRpartPlot` 出图漂亮
- 可用来处理非线性模型与变量选择

## Bagging

- 重采样 重新计算预测值
- 平均或投票给出结果
- 减少方差 偏差类似 适用于非线性过程
- bagged trees
  - 重采样
  - 重建树
  - 结果重评价
  - 更稳健 效果不如RF
- Bagged loess 可用来处理细节

## radom forest

- bootstrap采样
- 每一个节点bootstrap选取变量
- 多棵树投票
- 准确度高 速度慢 不好解释 容易过拟合

## boosting

- 弱预测变量加权后构建强预测变量
- 从一组预测变量开始
- 添加有惩罚项的预测变量来训练模型
- 以降低训练集误差为目的
- 通用方法

## 其他预测算法

- 参考[统计学习导论笔记](https://github.com/yufree/ISLRchnotes)

## 模型联合

- 通过平均与投票结合模型
- 联合分类器提高准确率
- `caretEnsemble` 包
- 案例 广义加性模型

```
library(ISLR); data(Wage); library(ggplot2); library(caret);
Wage <- subset(Wage,select=-c(logwage))
# Create a building data set and validation set
inBuild <- createDataPartition(y=Wage$wage,p=0.7, list=FALSE)
validation <- Wage[-inBuild,]; buildData <- Wage[inBuild,]
inTrain <- createDataPartition(y=buildData$wage,p=0.7, list=FALSE)
training <- buildData[inTrain,]; testing <- buildData[-inTrain,]
mod1 <- train(wage ~.,method="glm",data=training)
mod2 <- train(wage ~.,method="rf",data=training,trControl = trainControl(method="cv"),number=3)
pred1 <- predict(mod1,testing); pred2 <- predict(mod2,testing)
qplot(pred1,pred2,colour=wage,data=testing)
predDF <- data.frame(pred1,pred2,wage=testing$wage)
combModFit <- train(wage ~.,method="gam",data=predDF)
combPred <- predict(combModFit,predDF)
sqrt(sum((pred1-testing$wage)^2))
sqrt(sum((pred2-testing$wage)^2))
sqrt(sum((combPred-testing$wage)^2))
```

## 无监督预测

- 先聚类 后预测
- `clue` 包 `cl_predict` 函数
- 推荐系统

## 预测

- 时序数据 包含趋势 季节变化 循环
  - 效应分解 `decompose`
  - `window` 窗口
  - `ma` 平滑
  - `ets` 指数平滑
  - `forecast` 预测
- 空间数据同样有这种问题 临近依赖 地域效应
- `quantmod` 包 或 `quandl` 包处理金融数据 
- 外推要谨慎