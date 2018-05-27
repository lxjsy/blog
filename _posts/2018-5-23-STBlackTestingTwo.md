---
layout: post
title: "黑盒测试--决策表方法与因果图法"
date: 2018-5-22 18:41:12 
description: "黑盒测试中决策表方法和因果图方法"
tag: Software Testing
---

本章继续学习黑盒测试方法！

### 决策表方法
* 在所有的功能测试方法中，基于决策表的测试方法是最严格的，因为决策表本身具有严格的逻辑性
* 决策表是分析和表达多逻辑条件下执行不同操作的工具
* 决策表能够将列举各种复杂情况，使用决策表设计出的测试用例是完整，不会产生遗漏

#### 组成部分
决策表有四部分组成
* Stub portion(桩部分):left side
* Entry portion(条目部分):right side
* Condition portion(条件部分):up side
* Action portion(行动部分):bottom side

|  |  |
|--|--|
|condition stub|condition entry|
|action stub|action entry|

条目部分的一列是一条规则，规则指示在规则的条件部分中指示的条件环境下要采取的行动
决策表如果有N个条件，那么其有2^N条规则

#### 适用范围和限制
* IF-THEN-ELSE逻辑
* 输入变量逻辑关系
* 输入和输出之间的因果关系
* 涉及输入变量子集的计算

决策表不能很好地扩展，但是决策表可以进行迭代，有助于改进决策表

### 测试步骤
1. 确定规则数量
2. 列出所有的条件桩和行动桩
3. 填写条件实体
4. 填写行动实体
   4.1 如果有多条规则有相同的行动，且条件相似，可以进行合并，使用"-"表示不关心此条目
   4.2 不关心条目有两种：条件无关，或者条件不适用

#### 例题
> 1. Develop a decision table for the descriptions below
No charges are reimbursed（报销）to the patient until the deductible（扣除） has been met. After the deductible has been met, the amount to be reimbursed depends on whether or not the doctor or hospital is a "Preferred Provider." For preferred providers Doctor's office visits are reimbursed at 65% and Hospital visits are reimbursed at 95%. For other providers reimburse 50% for Doctor's Office visits or 80% for Hospital visits. 

Decison table

|choice\rule|1|2|3|4|5|
|-----------|-|-|-|-|-|
|C1：deductible?|F|T|T|T|T|
|C2:preferred providers?|-|T|T|F|F|
|C3:visit place?|-|H|D|H|D|
|A1:reimubursed at 95%|-|T|-|-|-|
|A2:reimubursed at 80%|-|-|-|T|-|
|A3:reimubursed at 65%|-|-|T|-|-|
|A4:reimubursed at 50%|-|-|-|-|T|
|A5:reimubursed at 0%|T|-|-|-|-|

> 2. Develop a decision table for the YesterDate function.

Y1 = {Year is a common year}<br>
Y2 = {Year is a leap year}<br>
M1 = {Month in 2, 4, 6, 8, 9, 11}<br>
M2 = {Month in 5, 7, 10, 12}<br>
M3 = {Month in 1}<br>
M4 = {Month in 3}<br>
D1 = {Day in 2 to 31}<br>
D2 = {Day in 1}

Decison table

|choice\rule|1|2|3|4|5|6|
|-----------|-|-|-|-|-|-|
|Month|M1 M2<br>M3 M4|M1|M2|M3|M4|M4|
|Day|D1|D2|D2|D2|D2|D2|
|Year|-|-|-|-|Y1|Y2|
|year - 1|-|-|-|T|-|-|
|month - 1|-|T|T|-|T|T|
|month set 12|-|-|-|T|-|-|
|day - 1|T|-|-|-|-|-|
|day set 31|-|T|-|T|-|-|
|day set 30|-|-|T|-|-|-|
|day set 29|-|-|-|-|-|T|
|day set 28|-|-|-|-|T|-|

### 因果图法
* 因果关系图是一种可视表示方法，展现输入和输出之间的逻辑关系，可以用布尔表达式来表示
* 因是需求中任何可能影响程序输出的条件
* 果是程序对某些组合输入条件的响应

#### 因果图表示方法
* Implication(等价)
IF C1 is 1, then E1 is 1 also,else E1 is 0.

![Implication](/images/ST/CauseEffectGraphing/Implication.png)

* Not(非)
IF C1 is 1, then E1 is 0,else E1 is 1.

![Not](/images/ST/CauseEffectGraphing/Not.png)

* Or(或)
if C1 or C2 or C3 is 1，then E1 is 1，else E1is 0."Or" may have arbitrary number of inputs.

![Or](/images/ST/CauseEffectGraphing/Or.png)

* And(与)
if both C1 and C2 are 1，then E1is1，else E1is 0. "And" may have arbitrary number of input.

![And](/images/ST/CauseEffectGraphing/And.png)

#### 约束
在实际问题中，输入状态存在某些依赖

* Exclusive()

![Exclusive](/images/ST/CauseEffectGraphing/Exclusive.png)

* Inclusive()

![Inclusive](/images/ST/CauseEffectGraphing/Inclusive.png)

* One and only one()

![Onlyone](/images/ST/CauseEffectGraphing/Onlyone.png)

* Requires()

![Require](/images/ST/CauseEffectGraphing/Require.png)

* Masking()

![Masking](/images/ST/CauseEffectGraphing/Masking.png)

### 因果图方法步骤
1. 规格说明文档确定原因和效果，并为其分配一个唯一的标识符（注意：效果也可能是其他影响的原因）
2. 使用因果图来表达原因与效果之间的关系
3. 将因果关系图转换为决策表
4. 通过决策表生产测试用例

#### 例题
> Cause-Effect Testing：In a given network, the sendfile command is used to send a file to a user on a different file server. The sendfile command takes three arguments: the first argument should be an existing file in the sender’s home directory, the second argument should be the name of the receiver’s file server, and the third argument should be the receiver’s userid. If all the arguments are correct, then the file is successfully sent; otherwise the sender obtains an error message. 

solution

![solve1](/images/ST/CauseEffectGraphing/solve1.jpg)

> 中国象棋走马下法：
> 1. 如果落点在棋盘外，则不移动棋子；
> 2. 如果落点与起点不构成日字形，则不移动棋子；
> 3. 如果落点处有自己放棋子，则不移动棋子；
> 4. 如果在落点方向的邻近交叉点有棋子（绊马腿），则不移动棋子；
> 5. 如果不属于1-4，且落点处无棋子，则移动棋子；
> 6.如果不属于1-4，且落点处为对方棋子（非老将），则移动棋子并除去对方棋子。
> 7. 如果不属于1-4，且落点处为对方老将，则移动棋子，并提示战胜对方，游戏结束。
> 请画出因果图，并转换成判定表

solution
![solve2](/images/ST/CauseEffectGraphing/solve2-1.jpg)
![solve3](/images/ST/CauseEffectGraphing/solve2-2.jpg)
不可能的情况没有列举出来，因为测试用例不需要用到该部分

### 正交矩阵测试方法
正交矩阵测试是一种系统的、统计的测试方法

#### 适用范围
* 用户界面测试
* 系统测试
* 回归测试
* 配置测试
* 性能测试

### 测试类型
正交矩阵测试方法主要有三种：综合测试、单因子测试、正交测试

#### 综合测试
将所有因子及其取值进行排列组合，总共有factors^levels次测试
综合测试可以识别出因子及其取值的关系，能够得到很好的测试结果，但是实验次数较多

#### 单因子测试
每一次循环中仅测试一个因子，固定除了测试因子以外的所有因子，每次循环结束时，选出最优的测试因子，加入到最优测试因子序列和下一次中，单因子测试实验测试为level+(factors-1)(levels-1)，较综合测试实验次数少且操作容易，但是如果因子之间存在依赖，结果可能有较大的差异

#### 正交测试
结合综合测试和单因子测试的优点，选择从综合测试表中典型的测试用例进行组合，最终得到表示为L<sub>Runs</sub>(Levels<sup>Factors</sup>)的正交表，实验测试为$\sum$ (level amount of every factor -1) + 1

### 实验步骤
* 根据规格说明文档找出所有的因子和取值
* 选择最合适（实验测试与最少测试次数相同或相近）的正交表
* 填入测试值，并写出结果

#### 例题
> Consider a web page with three distinct sections(Top, Midddle, and Bottom) that can be individually shown or hidden by the user. Please use orthogonal array testing to test the interactions of the different sections. 

solution 
最少实验次数=（2-1）+（2-1）+（2-1）+1 = 4
选择L<sub>4</sub>(2<sup>3</sup>)的正交矩阵实验

|choice\rule| Top | Middle | Bottom | excpepted result |
|-----------|-----|--------|--------|------------------|
|1|hidden|hidden|hidden|all hidden|
|2|hidden|shown|shown|only top hidden|
|3|shown|hidden|shown|only Middle hidden|
|4|shown|shown|hidden|only Bottom hideen|
