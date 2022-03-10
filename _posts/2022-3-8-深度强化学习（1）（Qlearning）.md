---
redirect_from: /_posts/2022-3-8-深度强化学习（1）（Qlearning）.md
title: 2022-3-8-深度强化学习（1）（Qlearning）
tags: 
  - 深度强化学习
  - 算法
---

### 部分理解和代码来自“莫烦python”

## 一、Qlearning介绍
&emsp;&emsp;Qlearning简单来说就是依照Q表（Q表就相当于我们的记忆和经验，我们在从小到大的过程中经历的各种事情所得到的经验，让我们在未来遇到事情能够更好的进行决策）进行决策的算法，这里有几个符号需要了解：状态集S, 动作集A, 即时奖励R，衰减因子γ, 探索率ϵ。
 
## 二、Qlearning基本流程
  <br/>算法输入：迭代轮数T，状态集S, 动作集A, 步长α，衰减因子γ, 探索率ϵ</br>
  <br/>输出：所有的状态和动作对应的价值Q</br>
　<br/>1. 随机初始化所有的状态和动作对应的价值Q. 对于终止状态其Q值初始化为0.</br>
     2. for i from 1 to T，进行迭代。</br>
     &emsp;a) 初始化S为当前状态序列的第一个状态。</br>
     &emsp;b) 用ϵ−贪婪法在当前状态S选择出动作A</br>
     &emsp;c) 在状态S执行当前动作A,得到新状态S′和奖励R</br>
     &emsp;d)  更新价值函数Q(S,A):Q(S,A)+α(R+γmaxaQ(S′,a)−Q(S,A))</br>
     &emsp;e) S=S′</br>
     &emsp;f) 如果S′是终止状态，当前轮迭代完毕，否则转到步骤b</br>

## 三、Qlearning例子帮助理解
   <br/>我们在生活中常常会面临学习和玩耍两种选择，假设一个q表：</br>

   | q表 | a1  |a2|
   |  ----  | ----  |----|
   | s1  | -2 | 1|
   | s2  | -4 | 2|

   <br/>在Qlearning基本流程中的R+γmaxaQ(S′,a)为Q现实，其中maxaQ(S′,a)就是从q表中取出下一个状态值最大的那个动作，比如状态s1时采取动作a2后，进入状态s2有一个奖励（注意这个奖励是自己设置的，并不等于q（s1,a2)),此时选择s2状态下q值最大的动作，利用R+γmaxaQ(S′,a)计算出Q现实，然后与Q表中的Q（s1，a2）做差，最终Q(S,A)=Q(S,A)+α(R+γmaxaQ(S′,a)−Q(S,A))
   
## 四、细节理解</br>
   1.ϵ−贪婪法是用在决策上的一种策略, 比如 epsilon = 0.9 时, 就说明有90% 的情况我会按照 Q 表的最优值选择行为, 10% 的时间使用随机选行为。</br>
   2.alpha是学习率, 来决定这次的误差有多少是要被学习的, alpha是一个小于1 的数。 </br>
   3.gamma 是对未来 reward 的衰减值。</br>
   ![gamma_decline](https://github.com/muzilyd/blog-image/blob/ea18023fb2dce0ab587d997e0eb4cc475d034195/reinforcement%20learning/gamma%20decline.png)</br>
   &emsp;&emsp;(1)gamma = 1 时,在 s1 看到的 Q 是未来没有任何衰变的奖励,系统能够看到以后所有的状态的价值</br>
   &emsp;&emsp;(2)gamma = 0 时,在 s1 只能看到眼前的reward
   &emsp;&emsp;(3)gamma 从 0 变到 1, 对远处的价值看得越清楚, 所以机系统随着gamma的增加，对未来的价值就越来越重视, 不仅仅只看眼前的利益, 也为未来的价值着想。</br>
