---
title: Java命名规范
toc: true
mathjax: true
date: 2021-01-02 10:38:01 +0800
tags: Java
categories: Code 
math: false
mermaid: false
---
1. 【强制】所有编程相关的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。
反例：```MAX_COUNT / EXPIRED_TIME```
7. 【强制】抽象类命名使用 Abstract 或 Base 开头；异常类命名使用 Exception 结尾，测试类命名以它要
测试的类的名称开始，以 Test 结尾。
8. 【强制】类型与中括号紧挨相连来定义数组。
正例：定义整形数组 int[] arrayDemo。
反例：在 main 参数中，使用 String args[] 来定义。
9. 【强制】POJO 类中的任何布尔类型的变量，都不要加 is 前缀，否则部分框架解析会引起序列化错误。
说明：本文 MySQL 规约中的建表约定第 1 条，表达是与否的变量采用 is_xxx 的命名方式，所以需要在 <resultMap> 设置从 is_xxx 到 xxx 的映射关系。
反例：定义为基本数据类型 Boolean isDeleted 的属性，它的方法也是 isDeleted()，框架在反向解析时，“误以为”对应的属性名称是 deleted，导致属性获取不到，进而抛出异常。
10. 【强制】包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。
正例：应用工具类包名为 com.alibaba.ei.kunlun.aap.util；类名为 MessageUtils（此规则参考 spring 的框架结构）。
11. 【强制】避免在子父类的成员变量之间、或者不同代码块的局部变量之间采用完全相同的命名，使可理解性降低。
说明：子类、父类成员变量名相同，即使是 public 也是能够通过编译，而局部变量在同一方法内的不同代码块中同名也是合法的，但是要避免使用。对于非 setter / getter 的参数名称也要避免与成员变量名称相同。
反例：

```java
public class ConfusingName {
    protected int stock;
    protected String alibaba;
    // 非 setter/getter 的参数名称，不允许与本类成员变量同名
    public void access(String alibaba) {
        if (condition) {
            final int money = 666;
            // ...
        }
        for (int i = 0; i < 10; i++) {
            // 在同一方法体中，不允许与其它代码块中的 money 命名相同
            final int money = 15978;
            // ...
        }
    }
}
class Son extends ConfusingName {
    // 不允许与父类的成员变量名称相同
    private int stock;
}
```

12. 【强制】杜绝完全不规范的英文缩写，避免望文不知义。
反例：AbstractClass“缩写”成 AbsClass；condition“缩写”成 condi；Function“缩写”成Fu，此类随意缩写严重降低了代码的可阅读性。
13. 【推荐】为了达到代码自解释的目标，任何自定义编程元素在命名时，使用完整的单词组合来表达。
正例：在 JDK 中，对某个对象引用的 volatile 字段进行原子更新的类名为AtomicReferenceFieldUpdater。
反例：常见的方法内变量为 int a; 的定义方式。
14. 【推荐】在常量与变量命名时，表示类型的名词放在词尾，以提升辨识度。
正例：```startTime / workQueue / nameList / TERMINATED_THREAD_COUNT```
反例：```startedAt / QueueOfWork / listName / COUNT_TERMINATED_THREAD```
15. 【推荐】如果模块、接口、类、方法使用了设计模式，在命名时要体现出具体模式。
说明：将设计模式体现在名字中，有利于阅读者快速理解架构设计思想。
正例：```public classOrderFactory;```
正例：```public class LoginProxy;```
正例：```public class ResourceObserver;```
16. 【推荐】接口类中的方法和属性不要加任何修饰符号（public 也不要加），保持代码的简洁性，并加上有效的 Javadoc 注释。尽量不要在接口里定义常量，如果一定要定义，最好确定该常量与接口的方法相关，并且是整个应用的基础常量。
正例：接口方法签名 void commit();
正例：接口基础常量 String COMPANY = "alibaba";
    反例：接口方法定义 public abstract void commit();
    说明：JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的默认实现。
17.  接口和实现类的命名有两套规则：
- 【强制】对于 Service 和 DAO 类，基于 SOA 的理念，暴露出来的服务一定是接口，内部的实现类用 Impl 的后缀与接口区别。
    正例：CacheServiceImpl 实现 CacheService 接口。
- 【推荐】如果是形容能力的接口名称，取对应的形容词为接口名（通常是 –able 结尾的形容词）。
    正例：AbstractTranslator 实现 Translatable。
18. 【参考】枚举类名带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开。说明：枚举其实就是特殊的常量类，且构造方法被默认强制是私有。
    正例：枚举名字为 ProcessStatusEnum 的成员名称：SUCCESS / UNKNOWN_REASON
19. 【参考】各层命名规约：
- Service / DAO 层方法命名规约：
    1. 获取单个对象的方法用 get 做前缀。
    2. 获取多个对象的方法用 list 做前缀，复数结尾，如：listObjects
    3. 获取统计值的方法用 count 做前缀。
    4. 插入的方法用 save / insert 做前缀。
    5. 删除的方法用 remove / delete 做前缀。
    6. 修改的方法用 update 做前缀。
- 领域模型命名规约：
    1. 数据对象：xxxDO，xxx 即为数据表名。
    2. 数据传输对象：xxxDTO，xxx 为业务领域相关的名称。
    3. 展示对象：xxxVO，xxx 一般为网页名称。
    4. POJO 是 DO / DTO / BO / VO 的统称，禁止命名成 xxxPOJO。