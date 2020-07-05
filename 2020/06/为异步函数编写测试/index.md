# 如何为异步函数写测试?


## 0x00 引言
我是以卖码为生的海门。今天想和大家探讨一下如何为异步函数编写单元测试。

在实际的编码过程中，会遇到很多的异步函数，比如网络请求，文件读取等。它与同步函数的不同点在于它的结果是由回调函数或 Promise 对象返回的，也就是说在调用异步函数之后，不会像同步函数那样，被调用时挂起，直到操作完成就会得到结果。

## 0x01 让我们开始吧
为了简单起见，这次的需求是：读取某一文件，并返回该文件的行数。

开始前，我会告诉我们需要解决的一个问题：
1. 由于异步函数的结果不会立刻返回，所以在测试中需要有一个东西来证明回调函数或 Promise 对象返回了结果；

假设你已经会为同步函数编写测试了【如果不会请先花几分钟看这篇 ![4 年后，我再次入门 TDD](https://yihaimen.github.io/2020/06/4%E5%B9%B4%E5%90%8E%E6%88%91%E5%86%8D%E6%AC%A1%E5%85%A5%E9%97%A8tdd/) 】，然后一口气就写了如下的代码：

```javascript
    // files.js
    import fs from 'fs';

    export const linesCount = (fileName, onSuccess, onError) => {
        const processFile = (err, data) => {
            if (err) {
                onError('unable to open file ' + fileName);
            } else {
                onSuccess(data.toString().split('\n').length);
            }
        };

        fs.readFile(fileName, processFile);
    };
```

```javascript
    // files.test.js
    import { linesCount, linesCountP } from '../src/files';

    describe('test server side callback', () => {
        it('should return correct lines count for a valid file', () => {
            const onSuccess = (data) => {
                try {
                    expect(data).toBe(-100);
                } catch (error) {
                    // nothing
                }
            };

            linesCount('src/files.js', onSuccess);
        });
    });
```

上边的单测尽然通过了，可我设置的是 -100 行，没有比单测有误更糟的情况了。出现该问题就是因为单测不知道这个回调断言什么时候操作完成，即没有等到回调中的断言完成就结束了，所以我们来解决这个问题，加一个标识来验证回调操作完成。

```javascript
// files.test.js
    import { linesCount, linesCountP } from '../src/files';

    describe('test server side callback', () => {
        it('should return correct lines count for a valid file', (done) => {
            const onSuccess = (data) => {
                try {
                    expect(data).toBe(-100);
                } catch (error) {
                    // nothing
                }
            };

            linesCount('src/files.js', onSuccess);
        });
    });

```

通过查阅 Jest 的文档，了解到 done 就是那个处理回调的标识，但此时我们去执行，会看到一个异步回调超时的错误（默认超时是5秒）。在固定的超时时间范围内，单测还是不知道回调中的断言什么时候完成的问题，所以我们需要将这个标识放到断言语句的后边。

```javascript
    // files.test.js
    import { linesCount, linesCountP } from '../src/files';

    describe('test server side callback', () => {
        it('should return correct lines count for a valid file', (done) => {
            const onSuccess = (data) => {
                try {
                    expect(data).toBe(-100);
                    done();
                } catch (error) {
                    done(error);
                }
            };

            linesCount('src/files.js', onSuccess);
        });
    });
```

这个时候你会看到 Expected: -100，Received: 25，到这儿就把 -100 改为 25 就结束了，但单测还没有写完，目前仅仅是做了一个正向测试。接下来就是编写反向测试和异步测试，基本方法与 4 年后，我再次入门 TDD 一样，只不过是需要为回调函数和断言语句后添加一个标识。

## 0x02 测试Promise对象
上边描述了如何为回调函数编写测试，目的是为了简单阐述如何测试异步函数。因为大家都知道回调地狱的问题，所以出现了 Promise 这一异步解决方案，下来就介绍如何测试 Promise 对象。

有 4 种方式，第 3 和 第 4 种是常用方式，这里只介绍第 4 种书写方式，当然4 种方式没有优劣之分，只有哪些对自己的测试更简单：
1、使用 done 和 Promise 组合
2、返回 Promise
3、使用 async await
4、使用 async await 和 Promise 组合

此处我的浏览器是支持 AsyncFunction 特性的，如果不支持需要选用其他方式进行测试。我们只需要在单测函数的第二个参数加上 async 关键字，并在断言语句前加上 await 关键字，还使用了 Jest 提供的 resolves 和 rejects 匹配器简化测试语句。最后，异步测试依然遵循 3As 模式。

```javascript
    it('should return correct lines count for valid file - with promise and async', async () => {
        await expect(linesCountP('src/files.js')).resolves.toBe(25);
    });

    it('should report error for an invalid file name - with promise and async', async () => {
        await expect(linesCountP('src/async/files.js')).rejects.toMatch('unable to open file src/async/files.js');
    });
```

## 0x03 总结
如果学会了为同步函数写测试的方法论，对异步函数编写测试会变得很简单。但我们依然需要面临一个现实问题 ~ 依赖，它的存在会让单测很难进行自动化，故下一次将与大家探讨如何进行重构且正确地处理依赖。

## 0x04 参考
* 《JavaScript测试驱动开发》
* Jest 官网

代码仓库：https://github.com/yihaimen/JestJsApp
B 站链接：https://space.bilibili.com/383362014
博客地址：https://yihaimen.github.io/

![搜一搜](https://s1.ax1x.com/2020/06/08/tWbbz8.png)
