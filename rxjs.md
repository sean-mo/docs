# RxJs 学习笔记

# 概述

事件总线、点击事件都是异步流。开发者可以观测这些异步流，并调用特定的逻辑对它们进行处理。使用Reactive如同开挂：你可以创建点击、悬停之类的任意流。通常流廉价（点击一下就出来一个）而无处不在，种类丰富多样：变量，用户输入，属性，缓存，数据结构等等都可以产生流。举例来说：微博回文（译者注：比如你关注的微博更新了）和点击事件都是流：你可以监听流并调用特定的逻辑对它们进行处理。

基于流的概念，Reactive赋予了你一系列神奇的函数工具集，使用他们可以合并、创建、过滤这些流。 一个流或者一系列流可以作为另一个流的输入。你可以_合并_
两个流，从一堆流中_过滤_你真正感兴趣的那一些，将值从一个流_映射_到另一个流。

流是包含了有时序，正在进行事件的序列，可以发射(emmit)值（某种类型）、错误、完成信号。流在包含按钮的浏览器窗口被关闭时发出完成信号。

我们异步地捕获发射的事件，定义一系列函数在值被发射后，在错误被发射后，在完成信号被发射后执行。有时，我们忽略对错误，完成信号地处理，仅仅关注对值的处理。对流进行监听，通常称为订阅，处理流的函数是观测者，流是被观测的主体。这就是观测者设计模式。



中文简述：http://hao.jser.com/archive/9081/?utm_source=tuicool&utm_medium=referral
RxJava概念描述：http://gank.io/post/560e15be2dca930e00da1083?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io

## Observable：可观察者，即被观察者
> 暴露异步和基于事件的推动基于数据源,可观测序列可观测的抽象，代表一个数据源可以观察到，同时推送数据到感兴趣的队列（推送到Observer）。
> 可创建或查询Observeable序列

```
// 推送订阅消息
Observable.prototype.subscribe =  observer => { ... }
// observer: { onNext: Function onError: Function onCompleted: function }
```
#### Cold HOT 描述
http://tankcong.com/2015/12/02/Cold-Hot-Observables/

- Cold Observable：冷序列订阅的值是不共享的，被订阅时才被动触发。（异步）
- HOT Observable：热序列订阅的值是在用户间共享的，主动发送到订阅，不管是否有订阅（同步）。也不管订阅何时开始，持续发送订阅。就如一场足球直播循环，第10分钟进入，就从第10分钟看起

### 函数介绍

#### Rx.Observable.create 
> 创建一个可观测序列（Observable），并提供订阅方法，返回observer

```
var source = Rx.Observable.create(observer => {
  // Yield a single value and complete
  observer.onNext(42);
  observer.onCompleted();

  // Any cleanup logic might go here
  return () => console.log('disposed')
});

var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted

subscription.dispose();

```



## Observer：观察者
> 代表一个观察者寄存器订阅一个感兴趣的人，观察者支持Next/Error/Complete

```
// 提供当前的通知值的函数
Observer.prototype.onNext = value => { ... };
// 提供一个错误处理函数
Observer.prototype.onError = error => { ... };
// 提供一个完成发送函数
Observer.prototype.onCompleted = () => { ... };
```
### 函数介绍

#### 创建一个observer

```
// Creates an observable sequence of 5 integers, starting from 1
var source = Rx.Observable.range(1, 5);

// Create observer
var observer = Rx.Observer.create(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// Prints out each item
var subscription = source.subscribe(observer);

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted
```

## Subjects

> 继承于 Observable 和 Observer, 均可使用它们的方法

- AsyncSubject: oncomplete通知前的最后一个值或错误收到通过OnError,发送给所有订阅的观察家。
- BehaviorSubject：代表一个值随着时间变化而改变。
- ReplaySubject：参数如果为2，则返回缓存最后2条数据
- subject：

```
// Every second
var source = Rx.Observable.interval(1000);

var subject = new Rx.Subject();

var subSource = source.subscribe(subject);

var subSubject1 = subject.subscribe(
    x => console.log('Value published to observer #1: ' + x),
    e => console.log('onError: ' + e.message),
    () => console.log('onCompleted'));

var subSubject2 = subject.subscribe(
    x => console.log('Value published to observer #2: ' + x),
    e => console.log('onError: ' + e.message),
    () => console.log('onCompleted'));

setTimeout(() => {
    // Clean up
    subject.onCompleted();
    subSubject1.dispose();
    subSubject2.dispose();
}, 5000);

// => Value published to observer #1: 0
// => Value published to observer #2: 0
// => Value published to observer #1: 1
// => Value published to observer #2: 1
// => Value published to observer #1: 2
// => Value published to observer #2: 2
// => Value published to observer #1: 3
// => Value published to observer #2: 3
// => onCompleted
// => onCompleted
```

## 运算符分类

### @ 创建 observable sequence
##### Rx.Observable.create(subscribe)

创建一个流 

##### Rx.Observable.defer(observableFactory) 

创建一个工厂函数流（流嵌套流） 

##### Rx.Observable.defer(observableFactory) 

创建一个工厂函数流（流嵌套流） 

### @ 将事件或异步模式转换为可观测序列（observable sequence）
- from
- fromArray
- fromCallback
- fromNodeCallback
- fromEvent
- fromEventPattern
- fromPromise
- of
- toArray
- toMap
- toPromise
- toSet

### @ 合并多个Observable为单个Observable（observable sequence）

- amb：返回最快的序列
- combineLatest(...args, [resultSelector])：合并多个source，如果不指定resultSelecotr则返回一个数组列表
- concat：合并流，例如concat(a,b)，则将b的结果追加到a后面
- startWith：往流前面添加值，类似于before
- merge：合并流。例如merge([1,2,3],[4,5,6])=[1,4,2,5,3,6]。按时序合并
- mergeAll: 合并多个流为一个流
- repeat
- withLatestFrom：与combineLatest类似，区别在于A.widthLastestFrom(B),CombineLatest(A,B)
- zip: 合并多个流的值，提供resultSelector来返回结果
- scan：累计
- pairs: 将对象转成数组

### @ 基于时间的运算符

- throttle： 返回指定时间内第一项的反射值
- debounce: 忽略指定时间呢的操作
- delay: 延迟
- interval
- timer：延时多久后执后事件，第2个参数如果指定，则代表每隔第2参数执行一次，第3个参数为超时时间，例如timer(5, 100) 即5秒后，每隔100秒执行一次
- sample: 间隔几秒（参数可为Observable）
- timeout：设置一个超时时间，如果超时则返回错误
- timestamp：返回的对象新增时间戳

### @ 过滤和选择值的观察序列
- concatMap/selectConcat：类似于map，返回一个Observable序列的映射。能保证顺序，最终使用concat来合并

```
var source = Rx.Observable.range(0, 5)
    .concatMap(function (x, i) {
        return Rx.Observable
            .interval(100)
            .take(x).map(function() { return i; });
    });

``` 
- filter/where: 过滤
- flatMap/selectMany: 和concatMap类似，建议采用flatMap。相当于先map再mergeAll。最终使用merge合并
- flatMapLatest: 返回最近一个的序列，如time A,time B。则返回time A
- skip:跳过上一条返回的指定几条数量数据观察流，只返回剩余的流，即跳过前几条，返回后几条
- skipLast：反向跳过上一条返回指定的几条数量数据观察流，即跳过后几条，返回前几条
- skipUntil: 跳过指定Observable触发的值
- skipWhile: 跳过指定条件 
- take: 保留前几项
- takeLast:保留最后一项
- takeWhile: 保留指定条件指定项
- takeUntil: 保留指定Observable触发的值

### @ 分组

- buffer: 指定observable指定分组

### @命令
- do: 执行一次observer
- doWhile: 执行条件

### @ 错误操作

- catch/onErrorResumeNext: 忽略序列中的错误，优选择catch
- finally: 终止事件流
- retry: 重试次数
- retryWhen: 重试条件



## 核心库

### Observable Methods

#### Rx.Observable.amb(...args)

执行时序序列中最快的一条流

#### Rx.Observable.catch(...args)

忽略时序序列中错误的流，继续执行下一个流

#### Rx.Observable.concat(...args)

合并流 `Rx.Observable.concat(source1, source2);`

#### Rx.Observable.create(subscribe)

创建一个流 

#### Rx.Observable.defer(observableFactory) ？？？

创建一个工厂函数流（流嵌套流） 

#### Rx.Observable.empty() 

创建空流

#### Rx.Observable.from(iterable, [mapFn], [thisArg], [scheduler]) 

通过Array/Set/Map/String 创建一个Observable 序列 

#### Rx.Observable.generate(initialState, condition, iterate, resultSelector, [scheduler])

 生成一个以initialState初始值的序列流

#### Rx.Observable.return(value, [scheduler])
#### Rx.Observable.just(value, [scheduler])

return与just功能一样，返回一个固定值的流

#### Rx.Observable.merge([scheduler], ...args)

合并流

#### Rx.Observable.mergeDelayError(...args)

合并流，确保流中的错误不影响流的序列，延迟最后显示错误

#### Rx.Observable.never()

返回一个永远不会被终止的流

#### Rx.Observable.of(...args)
 
 转换参数为流
 
#### Rx.Observable.onErrorResumeNext(...args)
 
 无视错误执行数据流，即使有错也不会显示

#### Rx.Observable.pairs(obj, [scheduler])

将对象转换成数据流，类似of

#### Rx.Observable.range(start, count, [scheduler])

获得start至count的流

#### Rx.Observable.repeat(value, [repeatCount], [scheduler])

重复value

#### Rx.Observable.throw(exception, [scheduler])

####  Rx.Observable.throwError(exception, [scheduler])

####  Rx.Observable.throwException(exception, [scheduler])

返回异常错误流

#### Rx.Observable.zip(...args, [resultSelector])

合并指定的序列，通过resultSelector更改合并后的结果


# 示例：

#### 获取点击次数：

http://jsfiddle.net/staltz/4gGgs/27/

使用buffer/merge/throttle

### 合并流
http://jsfiddle.net/staltz/8jFJH/48/

combineLatest/merge/startWith