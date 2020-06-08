---
weight: 2
title: "你真的会写UnitTest吗？"
date: 2020-06-07T01:30:05+08:00
lastmod: 2020-06-07T01:30:05+08:00
draft: false
author: "易海门"
authorLink: "https://yihaimen.github.io"
description: "通过学习到达真的会写UnitTest的目的"
categories: ["TestDrivingJSApp"]
tags: ["JavaScript", "TDD", "UnitTest"]
featuredImage: ""
featuredImagePreview: "https://s1.ax1x.com/2020/06/08/tWOAa9.jpg"
license: "Creative Commons Attribution 4.0 International license"
lightgallery: true
---

### 引子
上周和中仁聊到了 Test Double，可是对于那几个概念依然不是很清晰。于是对自己进行了灵魂拷问：你真的会写UnitTest吗？这其实包含两个意思：一个是会为业务代码写UnitTest，另一个是知道该怎么写UnitTest。最终，我被自己问住了。

### 查资料
通过查看并回顾相关资料，我暂时了解了以下概念：
1. 什么是测试？- 对代码功能的正确性验证
2. 为什么要测试？- 解决各种“灵异”事件（神出鬼没的BUG），保证代码质量，更好的还原需求，最重要的是要测交付价值 不写无用的测试
3. 有哪些测试？- 单元测试、集成测试、性能测试等
4. 单元测试中的一个单元指的是什么？- 书面说明是最小可测试单元。我认为可以是一个函数或者一个类。**期待大家的答案**
5. 单元测试有几种常用方法论？- TDD、BDD <方法论不重要，重要的是使用方法论的人>
   1. TDD：侧重点偏向开发，通过 Test Case 来提高代码的质量和设计
   2. BDD：由外到内的开发方式，先定义业务成果，再实现这些业务成果，最后转化为验收标准
6. TDD 步骤（三角法）
   1. 伪代码
   2. 真代码
   3. 重构代码
7. TDD 思想 - 测试先行，小步迭代，重构和持续反馈
8. 写好单元测试的原则 - F.I.R.S.T
   1. Fast - 测试必须快速
   2. Independent - 测试独立，如测试结果不依赖环境或运行顺序
   3. Repeatable - 可重复执行的纯函数
   4. Self-validating - 自动化校验
   5. Thorough and Timely - 尽量覆盖全部场景
9. 如何覆盖全部场景？- 自补白盒测试
10. 每个测试三段论
    1.  准备数据 - Given
    2.  执行待测函数 - When
    3.  断言结果 - Then
11. Test Double 有哪些？- 针对这个，我还是有些概论比较模糊，**期待大家的答案**
    1.  Dummy
    2.  Fake
    3.  Stub
    4.  Mock
    5.  Spy

### 结语
为了写好一个单元测试，需要知道和理解的不仅仅是上边那些，比如还需要有识别代码中坏味道的能力，好的代码书写习惯，准确快速的盲打手法，快捷键的使用等等。接下来，我将边学习边分享的方式与大家一道，最终到达**我真的会写 Unit Test** 的目的。
![tWbbz8.png](https://s1.ax1x.com/2020/06/08/tWbbz8.png)