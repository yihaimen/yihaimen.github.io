---
title: "从 0 到 1 TDD实战"
date: 2020-06-13T14:25:01+08:00
author: "易海门"
authorLink: "https://yihaimen.github.io"
description: "将学会如何及时地编写测试，并用这些测试来影响代码的设计"
featuredImage: ""
featuredImagePreview: ""
categories: ["TestDrivingJSApp"]
tags: ["JavaScript", "TDD", "Jest"]
license: "Creative Commons Attribution 4.0 International license"
draft: true
---
## 引言
上边文章我说了自己不会真正的写UnitTest，虽然当下就查了些概念性的资料，但“道理（高内聚，低耦合，模块化，好设计，已修改，好维护）都懂，依然过不好这一生”的感受令我难受，于是我勇敢地迈出了第一步。

## 故事
我现在所做的事情就是在老系统中加新的需求，改老的功能，且需要加上测试。亲身体验就是代码完成后真的很难编写出有效的测试了，这个时候更像是代码驱动测试，对代码的设计没有任何帮助，最多就是完成了所谓的测试覆盖率。

尽早编写测试，可以让我们从代码使用者的角度思考问题，进而设计代码。可能也会和我一样，刚开始不知道从哪里入手，这确实是一个很有挑战性的事情，毕竟大部分程序员是没有写过测试或真正会写测试的。此文将从如何开始编写测试，到测试驱动代码设计。在这之前，默认你会使用JavaScript和Jest，当然测试框架也可以使用你自己熟悉的，比如Mocha等。

## 实践
我们将从一个小的栗子（回文）着手热身，学习一下编写测试的基本原理。

### 第一步 创建测试套件和金丝雀测试 
测试套件：一组相关测试的集合，这组测试验证一个函数或者一组密切相关的函数的行为。
金丝雀测试：最简单的测试，为了验证开发环境是否正确安装。

通过查看Jest的文档，可以知道测试套件的关键字是describe，测试用例的关键字是it，它们都有两个参数即一个名称和一个函数，需要注意的是测试套件的名称和测试用例的名称都要简洁明了，且测试用例的名称要清楚表达测试目的和期望得到的结果，因为单测只是为了验证代码的正确行为，而我们的测试名称，描述、表达并记录了代码的行为，或者说原始需求。最后，为了让测试贴合应用，我们需要与团队中的其他成员沟通，搞清楚真正的需求是什么，这样才能编写出行为正确的代码。

```javascript
    // plaindrome.test.js
    describe('palindrome test', () => {
        it('should pass this canary test', () => {
            expect(true).toBe(true);
        });
    });
```

![FeedBack]()
如上图，则说明我们的环境安装成功，下班就开始编写回文项目了。

### 第二步 开始编写单元测试
在编写测试的时候，我们是有模式可以遵循的：
1. 3As模式：Arrange-Act-Assert
2. GWT表达式：Given-When-Then
不管是哪一种模式，其实都是这三步：准备数据-执行函数-断言结果。所以在每个单测中也是有这三部分代码的，为了让代码看着舒服，不同的部分之间用空行隔开，另外每个单测里边的代码也需要保持简短，比如能用一行代码解决的事，也就不需要这个三段论了；相反要是一个单测代码冗长复杂，它可能表达出了两种意思：一是测试的代码写的不好，二是待测函数的设计不行。

有的同学就会想，模式我是知道了，可具体需要写哪些类型的测试来驱动开发，完善代码设计呢？有以下三种：
1. 正向测试：当前置条件满足时，验证代码的结果确实符合预期
2. 反向测试：当前置条件或者输入不符合要求时（如：边界情况，非法输入），代码能优雅地进行处理，可以检查系统的容错能力和可靠性
3. 异常测试：代码在应该抛出异常的地方正确地抛出了异常

除了以上的模式和类型，我们还需要遵循F.A.I.R原则（有的称为F.I.R.S.T原则），大体意思就是说：
1. 测试必须快速，以便我们快速获取反馈
2. 自动化校验，解放双手
3. 相互独立，测试结果互不依赖
4. 可重复执行n次且结果一致

> Note：关注行为而非状态，避免为获取、设置状态的函数编写测试。从有业务价值的、有意思的行为开始编写。在这个过程中对那些必要的状态进行设置和获取。

首先编写单测之前，我们需要分析需求，拆解为任务列表，然后挑选一个任务转换成测试列表。我们可以将快速想到的用例记录下来，比如：
* mom是回文
* momu不是回文
这个列表并不是一蹴而就，而是在编码过程中还需要不断的完善它，因为我们会想到新的测试，不管怎么说，行动起来。

第一个测试用例：
```javascript
    // plaindrome.test.js 正向测试
    it('should return true for argument mom', () => {
        expect(isPalindrome('mom')).toBe(true);
    });
```
> 如果你还想测试同类型的数据，比如dad，需要在另外单独的测试中添加，避免在一个测试中放置多个独立的断言，可以尽量少包含几个密切相关的断言。

!(Issue)[]
我们发现这个单测没有通过，看下Jest给我们反馈的信息告诉我们 isPalindrome 这个函数我们还没有实现或者还没有导入到测试文件中来，接下来我们将实现它并导入到测试文件中。这个时候我们不要急于实现代码，可以使用初始测试驱动函数的接口设计，一遍让代码更具表现力和可读性。比如函数名是否符合代码规范，传参是否符合要求，返回值还可以是其他类型吗等。一旦接口确定下来，我们就可以编写最简单的实现了。
```javascript
    // plaindrome.js
    export const isPalindrome = (word) => {
        return true;
    } 
```
> Note：让测试驱动设计，应该能够让我们将想到的问题提出来，帮助发现细节，然后在这个过程中梳理出代码的接口，还有可能找出需求的缺陷。

!(Okay)[]
现在我们看到我们的单测是通过了，那我们开始验证另外一个数据。
```javascript
    // plaindrome.test.js 正向测试
    import { isPalindrome } from '../src/palindrome';
    it('should return false for argument momu', () => {
        expect(isPalindrome('momu')).toBe(false);
    });
```

!(Refactor)[]
发现报错，根据反馈说我们期望的结果是false，但是给我们返回的是true，这说明我们代码实现有问题了，此时就需要我们去重构。

```javascript
    // plaindrome.js
    export const isPalindrome = (word) => {
        return word.split('').reverse().join('') === word;
    } 
```
!(Okay)[]

总结一下上边编写一个单测到程序通过的过程，它是一条反馈环： Red - Green - Refactor。首先我们会编写一个失败的测试，设计好被测函数的名称和传参；然后就需要根据Jest给的反馈找到失败的原因，编写最少的代码或回退代码让单测通过；最后通过重构让代码更加完善。
我们之前可能是这样做的：
1. 在声明测试方法后，便开始写实现代码；
2. 写完“所有”的测试代码才开始写实现；（这种估计很少，毕竟都不知道怎么写测试吧）
3. 从不重构；

当然上边的两个单测并不能完整的驱动代码的完成，初始的单测更像一个引导，有助于形成函数的接口，我们需要添加额外的单测用来驱动代码的完成，实现具体的需求。

接下来，我们来编写几个反向测试，这时我在测试列表上写到：
* 空字符串不是回文
* 两个空格串不是回文

```javascript
    // plaindrome.js
    export const isPalindrome = (phrase) => {
        return phrase.trim().length > 0 && phrase.split('').reverse().join('') === phrase;
    }
```

```javascript
    // plaindrome.test.js 
    it('should return false when argument is an empty string', () => {
        expect(isPalindrome('')).toBe(false);
    });

    it('should return false for argument with only two spaces', () => {
        expect(isPalindrome('  ')).toBe(false);
    });
```
编写完正向和反向测试后，我们还没有到达交付要求，我们还必须要验证代码是否能够正确抛出异常，即接下来我们需要编写异常测试，这时我在测试列表上写到：
* 不传参，抛出参数非法异常
> 只有当代码以预期的方式失败时，异常测试才应该通过。
```javascript
    // plaindrome.js
    export const isPalindrome = (phrase) => {
        if (phrase === undefined) {
            throw new Error('Invalid argument')
        }

        return phrase.trim().length > 0 && phrase.split('').reverse().join('') === phrase;
    } 
```

```javascript
    // plaindrome.test.js 
    // 思考：为什么要将 isPalindrome 放在另一个函数里边？尽管不放也能通过。
    it('should throw an exception if argument is missing', () => {
        const call = () => {
            isPalindrome();
        };

        expect(call).toThrow('Invalid argument');
    });
```

至此，有了之前单测的保驾护航，我们可以试着重构代码：回头思考函数名是否合适，入参名称和类型还可以是其他的吗，具体实现是不是还有更舒服的写法。不管怎么说，现在是敢修改以前的代码了！！！

### 第三步 代码覆盖率报告
代码覆盖率报告是非常有价值的，也不要过度依赖它。它能快速标识出哪些代码没有被测试覆盖到，还可以扔给老板看。它实际的数值并不那么重要，糟糕的数值会引起我们的警觉，但良好的数值也不要太过高兴，我们更需要关注的是检查哪行代码没有被测试覆盖，同时确保在修改代码时覆盖率数值没有降低。但是代码都覆盖了也不一定是编写了充分的测试。我们可以从未覆盖的代码发现设计的缺陷，但我们不能从全部覆盖的代码上看到问题，所以我们还需要做代码和测试的审查来弥补。

![测试报告]()

### 总结
测试先行，有助于完善代码设计；小步迭代，有助于快速获取反馈重构代码。

本文代码：xxx
## 参考
* Jest官网
* 《JavaScript测试驱动开发》


