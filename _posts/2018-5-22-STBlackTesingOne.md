---
layout: post
title: "黑盒测试--等价类划分与边界值分析"
date: 2018-5-22 18:41:12 
description: "黑盒测试中等价类划分方法与边界值分析方法"
tag: Software Testing
---

### 黑盒测试
* 程序测试用例由系统规格说明文档（C1：specification）生成
* 测试计划可以在软件过程的早期进行
* 不深入代码细节的测试方法
* 主要针对软件界面和软件功能进行测试

### 优点
* 测试是公平的，因为设计人员和测试人员相互独立
* 测试人员不需要任何特殊编程语言知识
* 测试人员是从用户的角度出发，而非设计人员的角度
* 测试用例可以在规格文档完成后尽快设计出来

### 等价类划分方法
输入数据和输出结果通常属于不同的类，这些类中的成员都是有关联的。每个类都是一个等价部分或是程序的行为与类中成员相同的域。测试用例应该从每部分选择出来。

#### 等价类
等价类是输入域或者输出域的子集，每个等价类中的典型数据与其他数据应该相同。

* 有效等价类
它是规格说明文档中一个由合理而有意义的数据组成的集合，它可以用来检查应用程序是否实现了预期的功能和性能

* 无效等价类
它是规格说明文档中一个由不合理而无意义的数据组成的集合，它可以用来检查无效的数据是否正确处理。

#### 等级类划分原则
> 规定范围和个数，有效一个无效俩。
>
> 必须条件布尔量，有效无效一没错。
>
> 不同输入不同待，有效多来无效单。
> 
> 输入数据有限制，有效单一无效多。
>
> 等价划分综合断，类中有类细定度。

#### 例题
> Consider an application App that takes two inputs `name` and `age`, where name is a nonempty string containing at most 20 alphabetic(字母) characters and age is an integer that must satisfy the constraint 0≤age≤120. The App is required to display an error message if the input value provided for age is out of range. The application truncates any name that is more than 20-character in length and generates an error message if an empty string is supplied for name. 
（1）Please find out the equivalence classes.
（2）Construct test cases using the equivalence classes derived in（1）.

（1）等价类

| Input | Vaild equivalence class | Invaild equivalence class |
|-------|-------------------------|---------------------------|
|name|① 0 < length <= 120 <br> ② 字母字符| ③ length = 0 <br> ④ length >120 <br> ⑤ 含非字母字符|
|age|⑥ 0 <= age <= 120  <br> ⑦ 整数|⑧ age < 0 <br> ⑨ age > 120 <br> ⑩ 非整数|

（2）测试用例

|Test Case ID | name | age | excepted result|
|-------------|------|-----|----------------|
|1|google|60|normal operation|
|2|null|60|error:empty string|
|3|googleyahoomcrisoftware|60|name=googleyahoomcrisoftw|
|4|google6|60|error:string has number characters|
|5|google|-1|error:age out of range|
|6|google|121|error:age out of range|
|7|google|a|error:age isn't an integer|


### 边界值分析法
边界值分析关注于`输入空间`的边界来识别测试用例，其基本原理是一个输入变量的极值附近容易出现错误

#### 使用范围
被测试程序有几个独立的有界物理量，如温度、速度、负载等，如果存在依赖，可能效果不是很好需要用决策表技术进行解决

#### 取值
基于单缺陷错误，在可能的输入取值范围内，选取min-,min,min+,nom,max-,max,max+这7个取值。

### 总结
* 等价类测试的弱形式不如强形式测试全面
* 无效值会引起运行错误的时候（实现语言是强类型），则没有必要做健壮形式的测试。
* 错误条件很重要的时候，健壮测试很重要。
* 边界值测试是等价类测试的一种补充，两者结合可以加强测试效果。
* 要进行多次尝试，确认最合适的等价类划分。


#### 例题
> Consider a method fp, brief for findPrice, that takes two inputs code and qty. The item code is represented by the integer code and the quantity purchased by another integer variable qty. fp accesses a database to find and display the unit price, the description, and the total price of the item corresponding to code. fp is required to display an error message, and return, if  either of the two inputs is incorrect. Assuming that an item code must be in the range 99…999 and quantity in the range 1…100.
>
> Please give your test cases using boundary-value analysis.

（1）边界值取值
code = {98, 99, 100, 550, 998, 999, 1000}
qty = {0, 1, 2, 50, 99, 100, 101}

(2)测试用例
|Test Case ID | code | qty | excepted result|
|-------------|------|-----|----------------|
|1|98|50|input incorrect|
|2|99|50|display message|
|3|100|50|display message|
|4|550|50|display message|
|5|998|50|display message|
|6|999|50|display message|
|7|1000|50|input incorrect|
|8|550|0|input incorrect|
|9|550|1|display message|
|10|550|2|display message|
|11|550|50|display message|
|12|550|99|display message|
|13|550|100|display message|
|14|550|101|input incorrect|

转载请注明：[锡佳的博客](http://www.luxijia.top) » [点击阅读原文](http://www.luxijia.top/2018/05/STBlackTestingOne/)