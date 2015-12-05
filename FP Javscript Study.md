# 前言

本文是Javascript函数式编程指南的学习总结及知识点，讲的是JS编程的另一种范式函数式编程。


## 我们在做什么：

简单的示例：

```
// 原有代码
add(multiply(flock_b, add(flock_a, flock_c)), multiply(flock_a, flock_b));

// 应用同一律，去掉多余的加法操作（add(flock_a, flock_c) == flock_a）
add(multiply(flock_b, flock_a), multiply(flock_a, flock_b));

// 再应用分配律
multiply(flock_b, add(flock_a, flock_a));

```

## 函数是一等公民

```
// 太傻了
var getServerStuff = function(callback){
  return ajaxCall(function(json){
    return callback(json);
  });
};

// 这才像样
var getServerStuff = ajaxCall;

```

### 纯函数

> 纯函数是这样一种函数，即相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用。

副作用可能包括...

- 更改文件系统
- 往数据库插入记录
- 发送一个 http 请求
- 可变数据
- 打印/log
- 获取用户输入
- DOM 查询
- 访问系统状态
- 函数内部变量引用外部变量导致不可预料的变化

追求纯函数

可缓存性（Cacheable）

```
var memoize = function(f) {
  var cache = {};

  return function() {
    var arg_str = JSON.stringify(arguments);
    cache[arg_str] = cache[arg_str] || f.apply(f, arguments);
    return cache[arg_str];
  };
};
// 值得注意的一点是，可以通过延迟执行的方式把不纯的函数转换为纯函数
var pureHttpCall = memoize(function(url, params){
  return function() { return $.getJSON(url, params); }
});

```

可移植性／自文档化（Portable / Self-Documenting）

> 纯函数的依赖很明确，因此更易于观察和理解.
> 
> “面向对象语言的问题是，它们永远都要随身携带那些隐式的环境。你只需要一个香蕉，但却得到一个拿着香蕉的大猩猩...以及整个丛林”。

```
// 不纯的
var signUp = function(attrs) {
  var user = saveUser(attrs);
  welcomeUser(user);
};

// 纯的
var signUp = function(Db, Email, attrs) {
  return function() {
    var user = saveUser(Db, attrs);
    welcomeUser(Email, user);
  };
};
```

可测试性（Testable）

> Quickcheck——一个为函数式环境量身定制的测试工具。

合理性（Reasonable）

> 引用透明性（referential transparency）由于纯函数总是能够根据相同的输入返回相同的输出，所以它们就能够保证总是返回同一个结果，这也就保证了引用透明性

> “等式推导”：一对一替换

并行代码：

> 可使用webworker，并行执行代码，因为没有共享内存竞争。（考虑非纯函数复杂度不建议采用）

## 柯里化 Curry

```
var add = function(x) {
  return function(y) {
    return x + y;
  };
};

var increment = add(1);
var addTen = add(10);

increment(2);
// 3

addTen(2);
// 12

var curry = require('lodash').curry;

var match = curry(function(what, str) {
  return str.match(what);
});

match(/\s+/g, "hello world");
// [ ' ' ]

match(/\s+/g)("hello world");
// [ ' ' ]

var hasSpaces = match(/\s+/g);
// function(x) { return x.match(/\s+/g) }

hasSpaces("hello world");
// [ ' ' ]
```

推荐函数式编程库：

- https://github.com/lodash/lodash-fp
- http://ramdajs.com/

## Compose 函数组合

```
var compose = function(f,g) {
  return function(x) {
    return f(g(x));
  };
};
// f 和 g 都是函数，x 是在它们之间通过“管道”传输的值。

var toUpperCase = function(x) { return x.toUpperCase(); };
var exclaim = function(x) { return x + '!'; };
var shout = compose(exclaim, toUpperCase);
shout("send in the clowns");
//=> "SEND IN THE CLOWNS!"

```

> 结合律，符合结合律意味着不管你是把 g 和 h 分到一组，还是把 f 和 g 分到一组都不重要

```
// 结合律（associativity）
var associative = compose(f, compose(g, h)) == compose(compose(f, g), h);
// true

compose(toUpperCase, compose(head, reverse));
// 或者
compose(compose(toUpperCase, head), reverse);

// map 的组合律
var law = compose(map(f), map(g)) == map(compose(f, g));
```

> pointfree 模式指的是，永远不必说出你的数据。只关注函数组合（参数），不关注函数输入值

```
// 非 pointfree，因为提到了数据：word
var snakeCase = function (word) {
  return word.toLowerCase().replace(/\s+/ig, '_');
};

// pointfree
var snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);

snakeCase('word xx') // word_xx  
```

> debug 组合的一个常见错误是，在没有局部调用之前，就组合类似 map 这样接受两个参数的函数


```
var trace = curry(function(tag, x){
  console.log(tag, x);
  return x;
});

var dasherize = compose(join('-'), toLower, trace("after split"), split(' '), replace(/\s{2,}/ig, ' '));
// after split [ 'The', 'world', 'is', 'a', 'vampire' ]
```
> 范畴学（category theory）是数学中的一个抽象分支，能够形式化诸如集合论（set theory）、类型论（type theory）、群论（group theory）以及逻辑学（logic）等数学分支中的一些概念。范畴学主要处理对象（object）、态射（morphism）和变化式（transformation）

- 对象的搜集
- 态射的搜集: 态射是标准的、普通的纯函数。
- 态射的组合: compose 函数是符合结合律的组合
- identity 这个独特的态射:

> 所有的一元函数（unary function）（一元函数：只接受一个参数的函数） f 都成立

```
// identity
compose(id, f) == compose(f, id) == f;
// true
```

> 示例：

```
var images = _.compose(_.map(img), srcs);
var renderImages = _.compose(Impure.setHtml("body"), images);
var app = _.compose(Impure.getJSON(renderImages), url);

```

## Hindley-Milner 类型签名

> 最后一个是返回类型，前2个是接收参数。如下，接收regex，str，返回[string]

```
//  match :: Regex -> String -> [String]
var match = curry(function(reg, s){
  return s.match(reg);
});

```
> 接收一个参数，返回一个函数（接收一个字符，返回一个字符数组）

```
//  match :: Regex -> (String -> [String])
var match = curry(function(reg, s){
  return s.match(reg);
});

//  onHoliday :: String -> [String]
var onHoliday = match(/holiday/ig);

// 更复杂推导
//  replace :: Regex -> (String -> (String -> String))
var replace = curry(function(reg, sub, s){
  return s.replace(reg, sub);
});
```

>  缩小可能性范围 parametricity： 即缩小描述范围

```
// head :: [a] -> a
// reverse :: [a] -> [a]

```

> 自由定理 free theorems: 即只选择性描述

```
// head :: [a] -> a
compose(f, head) == compose(head, map(f));

// filter :: (a -> Bool) -> [a] -> [a]
compose(map(f), filter(compose(p, f))) == compose(filter(p), map(f));
```
> 类型约束

```
// sort :: Ord a => [a] -> [a]
// assertEqual :: (Eq a, Show a) => a -> a -> Assertion

// 两个约束：Eq 和 Show。它们保证了我们可以检查不同的 a 是否相等，并在有不相等的情况下打印出其中的差异

```

## 函数容器与错误处理

```
var Container = function(x) {
  this.__value = x;
}
Container.of = function(x) { return new Container(x); };
```

> functor 是实现了 map 函数并遵守一些特定规则的容器类型。通常我们称它为 Identity

```
// (a -> b) -> Container a -> Container b
Container.prototype.map = function(f){
  return Container.of(f(this.__value))
}
Container.of(2).map(function(two){ return two + 2 })
//=> Container(4)
Container.of("flamethrowers").map(function(s){ return s.toUpperCase() })
//=> Container("FLAMETHROWERS")
Container.of("bombs").map(concat(' away')).map(_.prop('length'))
//=> Container(10)
```
> maybe，其实就是第一个参数为值，把值往后面边函数参数传。前提是该值可能传入null等意外值，需对值进行处理

```
//  map :: Functor f => (a -> b) -> f a -> f b
var map = curry(function(f, any_functor_at_all) {
  return any_functor_at_all.map(f);
});

//  withdraw :: Number -> Account -> Maybe(Account)
var withdraw = curry(function(amount, account) {
  return account.balance >= amount ?
    Maybe.of({balance: account.balance - amount}) :
    Maybe.of(null);
});

var Maybe = function(x) {
  this.__value = x;
}

Maybe.of = function(x) {
  return new Maybe(x);
}

Maybe.prototype.isNothing = function() {
  return (this.__value === null || this.__value === undefined);
}

Maybe.prototype.map = function(f) {
  return this.isNothing() ? Maybe.of(null) : Maybe.of(f(this.__value));
}

Maybe.of(null).map(match(/a/ig));
//=> Maybe(null)

Maybe.of({name: "Boris"}).map(_.prop("age")).map(add(10));

```

> 释放容器的值

```
//  maybe :: b -> (a -> b) -> Maybe a -> b
var maybe = curry(function(x, f, m) {
  return m.isNothing() ? x : f(m.__value);
});

//  getTwenty :: Account -> String
var getTwenty = compose(
  maybe("You're broke!", finishTransaction), withdraw(20)
);

getTwenty({ balance: 200.00});
// "Your balance is $180.00"

getTwenty({ balance: 10.00});
// "You're broke!"
```

> 错误处理: 用left来处理错误，用right返回结果

```
Left.prototype.map = function(f) { 
// 返回自身，如果是函数名为error，则相当于返回error('xxxx') = left('xxxx')
  return this;
}

Right.prototype.map = function(f) {
  return Right.of(f(this.__value));
}
//  getAge :: Date -> User -> Either(String, Number)
var getAge = curry(function(now, user) {
  var birthdate = moment(user.birthdate, 'YYYY-MM-DD');
  if(!birthdate.isValid()) return Left.of("Birth date could not be parsed");
  return Right.of(now.diff(birthdate, 'years'));
});

getAge(moment(), {birthdate: '2005-12-12'});
// Right(9)

getAge(moment(), {birthdate: '20010704'});
// Left("Birth date could not be parsed")
```

> 通俗点来讲，一个函数在调用的时候，如果被 map 包裹了，那么它就会从一个非 functor 函数转换为一个 functor 函数。我们把这个过程叫做 lift。

```
//  either :: (a -> c) -> (b -> c) -> Either a b -> c
var either = curry(function(f, g, e) {
  switch(e.constructor) {
    case Left: return f(e.__value);
    case Right: return g(e.__value);
  }
});
```

> iO functor

```
var IO = function(f) {
  this.__value = f;
}

IO.of = function(x) {
  return new IO(function() {
    return x;
  });
}

IO.prototype.map = function(f) {
  return new IO(_.compose(f, this.__value));
}

//  io_window_ :: IO Window 返回 io.__value = function(){ return window }
var io_window = new IO(function(){ return window; });


// this.__value(fun(window))
io_window.map(function(win){ return win.innerWidth });
// IO(1430)
// 

io_window.map(_.prop('location')).map(_.prop('href')).map(split('/'));
// IO(["http:", "", "localhost:8000", "blog", "posts"])


//  $ :: String -> IO [DOM]
var $ = function(selector) {
  return new IO(function(){ return document.querySelectorAll(selector); });
}

$('#myDiv').map(head).map(function(div){ return div.innerHTML; });
// IO('I am some inner html')
```

## Monad

> pointed functor 是实现了 of 方法的 functor。“默认最小化上下文”这个术语可能不够精确，但是却很好地传达了这种理念：我们希望容器类型里的任意值都能发生 lift，然后像所有的 functor 那样再 map 出去。

```
O.of("tetris").map(concat(" master"));
// IO("tetris master")

Maybe.of(1336).map(add(1));
// Maybe(1337)

Task.of([{id: 2}, {id: 3}]).map(_.prop('id'));
// Task([2,3])

Either.of("The past, present and future walk into a bar...").map(
  concat("it was tense.")
);
// Right("The past, present and future walk into a bar...it was tense.")
```


## applicative functor

```
// 这样是行不通的，因为 2 和 3 都藏在瓶子里。
add(Container.of(2), Container.of(3));
//NaN

// 使用可靠的 map 函数试试
var container_of_add_2 = map(add, Container.of(2));
// Container(add(2))

Container.prototype.ap = function(other_container) {
  return other_container.map(this.__value);
}

Container.of(add(2)).ap(Container.of(3));
// Container(5)

// all together now
Container.of(2).map(add).ap(Container.of(3));
// Container(5)

```


2015-12-05: Monad functor 实在是太复杂了，冷静后再多看几遍
