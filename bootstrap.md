# R-study-notes_bootstrap
这是我学机器学习写作业时的笔记。我还不太会用GitHub，我在想要不就把笔记写在这个md文档里吧。

写bootstrap的两种方式。
第一种，来自博客https://www.r-bloggers.com/lang/chinese/540。
+ 写一个函数，求取统计量
+ 将此函数放入boot()进行运算
+ boot.ci() 求取置信区间。

举例：
```
xxx = function(data,indices){
...
}
```
> 这就是我们求的统计量
--------
```
library(boot)
set.seed(1) # 因为我们要抽取样本了，如果不保存seed每次抽到的样本会不一样()
result = boot(data=你要用的data, statistic = xxx, R = 1000) # 抽了1000次
```
--------
```
boot.ci(result,conf=0.95,type=c('perc','bca'))
```
> conf表示置信水平，type表示了用何种算法来求区间，perc即使用百分位方法，bca表示adjusted bootstrap percentile，即对偏差进行了调整。(来自上文链接里那个博客)。


第二种，来自我老师上课的笔记。
> 第一步，写一个函数求取统计量
```
xxx = function(data){
...
}
xxx(data数据名)
```
-------
```
set.seed(1)
sim_list = replicate(n = 1000,data数据名[sample(1:100,replace=T),], simplify=F) #抽样1000次
yyy = unlist(lapply(sim_list,xxx))
```

> 我在网上查了下没看懂这两行代码，lapply返回的是list, unlist()之后返回的是向量，那为什么不直接用sapplly()返回向量呢？
> 上面这行代码应该是将sim_list数据放进xxx函数再得到yyy
-------
```
quantile(alpha.boot,probs=c(0.025,0.975))#得到了95%置信区间数据
```
