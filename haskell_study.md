# haskell 趣学指南小结

安装开发环境(mac)：

``` $ brew install ghc cabal-install ```

进入haskell REPL: `ghci`

- 帮助：`:?`
- 载入：`:l [filename]`
- 重载：`:reload`
- 退出： `:quit`
## 函数

- 内置函数

`succ` 值加1， `min` 取最小值, `max` 取最大值

```
ghci> succ 9 + max 5 4 + 1
16
ghci> (succ 9) + (max 5 4) + 1
16
```
- 创建函数
 
 1、新建一个文件`doubleUs.hs`，保存以下内容
 
 ```
 doubleUs x y = x + x + y + y
 ```
 2、装载该文件 `:l filename`

   ```
	:l doubleUs.hs
	
	```
 3、执行函数

	```
	ghci> doubleUs 4 9
	26
	ghci> doubleUs 2.3 34.2
	73.0
	```
4、`if`: `doubleSmallNumber x = (if x > 100 then x else x*2) + 1`

## List

- 定义一个LIST：`let lostNumbers = [4,8,15,16,23,48]`
- 字符串是LIST：`['w','o'] ++ ['o','t'] ` 等于 `woot`
- LIST 可以装LIST：`let b = [[1,2,3,4],[5,3,3,3],[1,2,2,3,4],[1,2,3]]`
- LIST可以比较：`[3,2,1] > [2,10,100]  ` 值类型必须是一致的

常见LIST函数：

- head 返回首个元素： `head [5,4,3,2,1]` 等于 `5`
- tail 去掉尾部返回： `tail [5,4,3,2,1]` 等于 `[4,3,2,1]`
- last 返回最后元素： `last [5,4,3,2,1]` 等于 `1`
- init 去掉最后元素： `init [5,4,3,2,1]` 等于 `[5,4,3,2]`
- length 返回长度：`length [3,2,5]` 等于 `3`
- null 非空列表：`null []` 等于 `True`
- reverse 反转列表：`reverse [3,2,1]` 等于 `[1,2,3]`
- take n 取前n个元素：`take 2 [2,3,5,3]` 等于 `[2,3]`
- minimum 最小值：`minimum [8,4,2,1,5,6]` 等于 `1`
- maximum 最大值： `maximum [1,9,2,3,4]`等于 `9`
- sum 求和：`sum [5,2,1,6,3,2,5,7] ` 等于 `31`
- product 求积： `product [6,2,1,2] ` 等于 `24`
- elem 包含：4 ``` `elem` ``` [3,4,5,6] ` 等于 True

## Range
使用`..`返回自然数、字母范围的LIST

- `[2,4..20]` `[3,6..20]` `['a'..'z']` `['K'..'Z']  `
- `take 10 (cycle [1,2,3])` `cycle`生成无限`1,2,3`，`take 10` 只返回前10个
- `replicate 3 10` 返回 `[10 10 10]`：返回3个重复的10。`repeat 5` 则返回无限个5

## LIST 理解

`[x*2 | x <- take 20 (repeat 5) , x*2 >= 2]`  [返回值 | 条件1, 条件2]

定义函数：odd代表是否奇数

`boomBangs xs = [ if x < 10 then "BOOM!" else "BANG!" | x <- xs, odd x]` 
`boomBangs [7..13]`

也可以加多个限制条件。若要达到 10 到 20 间所有不等于 13，15 或 19 的数，可以这样

`[ x | x <- [10..20], x /= 13, x /= 15, x /= 19`

多个参数求积：

`[ x*y | x <- [2,5,10], y <- [8,10,11]]`

更多示例：

```
ghci> let nouns = ["hobo","frog","pope"]
ghci> let adjectives = ["lazy","grouchy","scheming"]
ghci> [adjective ++ " " ++ noun | adjective <- adjectives, noun <- nouns]
["lazy hobo","lazy frog","lazy pope","grouchy hobo","grouchy frog", "grouchy pope","scheming hobo",
"scheming frog","scheming pope"]
```
定义length, `_`代表不在乎返回值

```
length' xs = sum [1 | _ <- xs]
```

嵌套：

```
ghci> let xxs = [[1,3,5,2,3,1,2,4,5],[1,2,3,4,5,6,7,8,9],[1,2,4,2,1,6,3,1,3,2,3,6]]
ghci> [ [ x | x <- xs, even x ] | xs <- xxs]

```

## 元组 Tuple

> Tuple 则要求你对需要组合的数据的数目非常的明确，它的型别取决于其中项的数目与其各自的型别。 Tuple 中的项由括号括起，并由逗号隔开。

**以下元素型别一样，但元素数组不一致，也导致错误的。**

```
[(1, 2), (8, 11, 5), (4, 5)]
```
**仅允许二元组使用的函数**

- fst 返回序列首项： `fst (8,11)` 等于 `8`
- snd 返回序列尾项： `snd ("wow", False)` 等于 `False`

**zip：将两个数组交叉成一组序地的LIST**

```
ghci> zip [1,2,3,4,5] [5,5,5,5,5]
[(1,5),(2,5),(3,5),(4,5),(5,5)]
ghci> zip [1 .. 5] ["one", "two", "three", "four", "five"]
[(1,"one"),(2,"two"),(3,"three"),(4,"four"),(5,"five")]
ghci> zip [1..] ["apple", "orange", "cherry", "mango"]
[(1,"apple"),(2,"orange"),(3,"cherry"),(4,"mango")]
```
**它添加一个限制条件，令其必须为直角三角形。同时也考虑上 b 边要短于斜边，a 边要短于 b 边情况**

```
ghci> let rightTriangles' = [ (a,b,c) | c <- [1..10], b <- [1..c], a <- [1..b], a^2 + b^2 == c^2, a+b+c == 24]
ghci> rightTriangles'
[(6,8,10)]
```

## 类型
`:t` 检测表达式型别。例：`:t 'a'`. 

同时也可以查询函数的类型，如 `:t fst` 等于 fst :: (a, b) -> a

`::`  类型推导， 例：

```
removeNonUppercase :: [Char] -> [Char]  
removeNonUppercase st = [ c | c <- st, c `elem` ['A'..'Z']]
addThree :: Int -> Int -> Int -> Int  
addThree x y z = x + y + z
```
**类型**

- Int ：表示整数。Int 是有界的，也就是说它由上限和下限。对 32 位的机器而言，上限一般是 2147483647，下限是 -2147483648。
- Integer： 无界整数，无上限
- Float： 浮点数
- Double: 双精度
- Bool: 布尔
- Char：字符

## TypeClasses

> 函数型别声明

```
ghci> :t (==)  
(==) :: (Eq a) => a -> a -> Bool
```

`Eq` 提供判断相等的接口 `==`和`=/`。如`:t (==)`

`Ord` 提供比较大小的型别. `compare` 函数取两个 `Ord` 类中的相同型别的值作参数，回传比较的结果。这个结果是如下三种型别之一：GT, LT, EQ。

```
ghci> :t (>)  
(>) :: (Ord a) => a -> a -> Bool
ghci> "Abrakadabra" < "Zebra"  
True  
ghci> "Abrakadabra" `compare` "Zebra"  
LT  
ghci> 5 >= 2  
True  
ghci> 5 `compare` 3  
GT
```
`Show`是字符串表示型别。

```
ghci> show 3  
"3"  
ghci> show 5.334  
"5.334"  
```
`Read` 与 `Show` 相反，将字符串转成对应的型别。同时必须提供两个参数进行运算（其中一个参数必须是字符型）

```
ghci> read "True" || False  
True  
ghci> read "8.2" + 3.8  
12.0  
ghci> read "5" - 2  
3  
ghci> read "[1,2,3,4]" ++ [3]  
[1,2,3,4,3]
```

`Enum`是枚举型别（即连续内容）。该 Typeclass 包含的型别有：(), Bool, Char, Ordering, Int, Integer, Float 和 Double。

```
ghci> ['a'..'e']  
"abcde"  
ghci> [LT .. GT]  
[LT,EQ,GT]  
ghci> [3 .. 5]  
[3,4,5]  
ghci> succ 'B'  
'C'
```
`Bounded` 的成员都有一个上限和下限。

```
ghci> minBound :: Int  
-2147483648  
ghci> maxBound :: Char  
'\1114111'  
ghci> maxBound :: Bool  
True  
ghci> minBound :: Bool  
False
ghci> maxBound :: (Bool, Int, Char)  
(True,2147483647,'\1114111')
```

## 模式匹配

> 可代替 `if-else` / `switch` 的模式匹配

满足表达式条件，否则 x

```
lucky :: (Integral a) => a -> String  
lucky 7 = "LUCKY NUMBER SEVEN!"  
lucky x = "Sorry, you're out of luck, pal!"
```

递归

```
factorial :: (Integral a) => a -> a  
factorial 0 = 1  
factorial n = n * factorial (n - 1)
```
Tuple可以使用模式匹配

```
addVectors :: (Num a) => (a, a) -> (a, a) -> (a, a)  
addVectors (x1, y1) (x2, y2) = (x1 + x2, y1 + y2)
addVectors (2,3) (3,6)
(5,9)
```
List Comprehension

```
ghci> let xs = [(1,3), (4,3), (2,4), (5,3), (5,6), (3,1)]  
ghci> [a+b | (a,b) <- xs]  
[4,7,6,8,11,4]
```
x:xs 该模型匹配应用很广，特别是递归模型. 

如：(x:_) ＝ (7,8) _仅能代表一个元素，如(1,3,5) 相当于 x=1, _= (3,5)
假如长度不确定，则（x:y:[])代表（1,2,...)


```
head' :: [a] -> a  
head' [] = error "Can't call head on an empty list, dummy!"  
head' (x:_) = x
ghci> head' [4,5,6]  
4  
ghci> head' "Hello"  
'H'
```
多个匹配,一般数组最后都会有个[],如果[1,2,3]，则一定会是1:2:3:[]

```
tell :: (Show a) => [a] -> String  
tell [] = "The list is empty"  
tell (x:[]) = "The list has one element: " ++ show x  
tell (x:y:[]) = "The list has two elements: " ++ show x ++ " and " ++ show y  
tell (x:y:_) = "This list is long. The first two elements are: " ++ show x ++ " and " ++ show y
*Main> tell []
"The list is empty"
*Main> tell [1]
"The list has one element: 1"
*Main> tell [1,2,3,5,6]
"This list is long. The first two elements are: 1 and 2"
```

例如 Length, 结果相当于1+(1+(1+0))。 例如 ham = (_:xs) 相当于 匹配 ham 

```
length' :: (Num b) => [a] -> b  
length' [] = 0  
length' (_:xs) = 1 + length' xs
```
`as` 模式，就是将一个名字 @置于模式前，如ok@ 则相当于 ok 则保留整体的引用

```
capital :: String -> String  
capital "" = "Empty string, whoops!"  
capital all@(x:xs) = "The first letter of " ++ all ++ " is " ++ [x]

ghci> capital "Dracula"  
"The first letter of Dracula is D"
```

## guard

> 类型于 `if-else`，不过带条件判断

```
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"  
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"  
    | otherwise                 = "You're a whale, congratulations!"
```

## 关键字WHERE

> 将重复的条件用WHERE代替

**存在重复的条件**

```
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | weight / height ^ 2 <= 18.5 = "You're underweight, you emo, you!"  
    | weight / height ^ 2 <= 25.0 = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | weight / height ^ 2 <= 30.0 = "You're fat! Lose some weight, fatty!"  
    | otherwise                   = "You're a whale, congratulations!"
```

**WHERE解决重复的条件**

```
bmiTell :: (RealFloat a) => a -> a -> String  
bmiTell weight height  
    | bmi <= skinny = "You're underweight, you emo, you!"  
    | bmi <= normal = "You're supposedly normal. Pffft, I bet you're ugly!"  
    | bmi <= fat    = "You're fat! Lose some weight, fatty!"  
    | otherwise     = "You're a whale, congratulations!"  
    where bmi = weight / height ^ 2  
          skinny = 18.5  
          normal = 25.0  
          fat = 30.0

也可以改成
where bmi = weight / height ^ 2  
      (skinny, normal, fat) = (18.5, 25.0, 30.0)
```

姓名的首字母

```
initials :: String -> String -> String  
initials firstname lastname = [f] ++ ". " ++ [l] ++ "."  
    where (f:_) = firstname  
          (l:_) = lastname
result:
initials "king" "soft"
"k. s."
```

where 函数

```
calcBmis :: (RealFloat a) => [(a, a)] -> [a]  
calcBmis xs = [bmi w h | (w, h) <- xs] 
    where bmi weight height = weight / height ^ 2
```

## 关键字 Let

> let x＝5 in x*x 代表定义一个x=5，in即执行 5 * 5

```
ghci> (let a = 100; b = 200; c = 300 in a*b*c, let foo="Hey "; bar = "there!" in foo ++ bar)  
(6000000,"Hey there!")
ghci> [let square x = x * x in (square 5, square 3, square 2)]  
[(25,9,4)]
``` 

可代替 `where`

```
calcBmis :: (RealFloat a) => [(a, a)] -> [a]  
calcBmis xs = [bmi | (w, h) <- xs, let bmi = w / h ^ 2]

```
允许忽略 `in`

```
ghci> let zoot x y z = x * y + z  
ghci> zoot 3 9 2  
29  
ghci> let boot x y z = x * y + z in boot 3 4 2  
14  
ghci> boot  
< interactive>:1:0: Not in scope: `boot'
```

## Case expressions

case表达式

```
case expression of pattern -> result  
                   pattern -> result  
                   pattern -> result  
                   ...
```
函数参数的模式匹配只能在定义函数时使用，而 case 表达式可以用在任何地方。例如

```
describeList :: [a] -> String  
describeList xs = "The list is " ++ case xs of [] -> "empty."  
                                               [x] -> "a singleton list."   
                                               xs -> "a longer list."

```

以下内容也是等价的

```
describeList :: [a] -> String  
describeList xs = "The list is " ++ what xs  
    where what [] = "empty."  
          what [x] = "a singleton list."  
          what xs = "a longer list."
```

## 递归

实现 maximum

```
maximum' :: (Ord a) => [a] -> a  
maximum' [] = error "maximum of empty list"  
maximum' [x] = x  
maximum' (x:xs)   
    | x > maxTail = x  
    | otherwise = maxTail  
    where maxTail = maximum' xs
```

```
replicate' :: (Num i, Ord i) => i -> a -> [a]  
replicate' n x  
    | n <= 0    = []  
    | otherwise = x:replicate' (n-1) x
```

## 高端函数

取一个函数和值，连续调用函数2次运算值

```
applyTwice :: (a -> a) -> a -> a  
applyTwice f x = f (f x)

ghci> applyTwice (+3) 10  
16  
ghci> applyTwice (++ " HAHA") "HEY"  
"HEY HAHA HAHA"  
ghci> applyTwice ("HAHA " ++) "HEY"  
"HAHA HAHA HEY"  
ghci> applyTwice (multThree 2 2) 9  
144  
ghci> applyTwice (3:) [1]  
[3,3,1]
```

取一个函数，2个LIST，函数运算2个LIST

```
zipWith' :: (a -> b -> c) -> [a] -> [b] -> [c]  
zipWith' _ [] _ = []  
zipWith' _ _ [] = []  
zipWith' f (x:xs) (y:ys) = f x y : zipWith' f xs ys

ghci> zipWith' (+) [4,2,5,6] [2,6,2,3]  
[6,8,7,9]  
ghci> zipWith' max [6,3,2,1] [7,3,1,5]  
[7,3,2,5]  
ghci> zipWith' (++) ["foo "，"bar "，"baz "] ["fighters"，"hoppers"，"aldrin"]  
["foo fighters","bar hoppers","baz aldrin"]  
ghci> zipWith' (*) (replicate 5 2) [1..]  
[2,4,6,8,10]  
ghci> zipWith' (zipWith' (*)) [[1,2,3],[3,5,6],[2,3,4]] [[3,2,2],[3,4,5],[5,4,3]]  
[[3,4,6],[9,20,30],[10,12,12]]
```

## map 与 filter

**map**

```
map :: (a -> b) -> [a] -> [b]  
map _ [] = []  
map f (x:xs) = f x : map f xs

ghci> map (+3) [1,5,3,1,6]  
[4,8,6,4,9]  
ghci> map (++ "!") ["BIFF"，"BANG"，"POW"]  
["BIFF!","BANG!","POW!"]  
ghci> map (replicate 3) [3..6]  
[[3,3,3],[4,4,4],[5,5,5],[6,6,6]]  
ghci> map (map (^2)) [[1,2],[3,4,5,6],[7,8]]  
[[1,4],[9,16,25,36],[49,64]]  
ghci> map fst [(1,2),(3,5),(6,3),(2,6),(2,5)]  
[1,3,6,2,2]
```

**filter**

```
filter :: (a -> Bool) -> [a] -> [a]  
filter _ [] = []  
filter p (x:xs)   
    | p x       = x : filter p xs  
    | otherwise = filter p xs

ghci> filter (>3) [1,5,3,2,1,6,4,3,2,1]  
[5,6,4]  
ghci> filter (==3) [1,2,3,4,5]  
[3]  
ghci> filter even [1..10]  
[2,4,6,8,10]  
ghci> let notNull x = not (null x) in filter notNull [[1,2,3],[],[3,4,5],[2,2],[],[],[]]  
[[1,2,3],[3,4,5],[2,2]]  
ghci> filter (`elem` ['a'..'z']) "u LaUgH aT mE BeCaUsE I aM diFfeRent"  
"uagameasadifeent"  
ghci> filter (`elem` ['A'..'Z']) "i lauGh At You BecAuse u r aLL the Same"  
"GAYBALLS"
```

## lambda 

> 其实就是匿名函数

lambda 是个表达式，因此我们可以任意传递。表达式 (\xs -> length xs > 15) 回传一个函数，它可以告诉我们一个 List 的长度是否大于 15。

```
numLongChains :: Int  
numLongChains = length (filter (\xs -> length xs > 15) (map chain [1..100]))


flip' :: (a -> b -> c) -> b -> a -> c  
flip' f = \x y -> f y x
```

## 关键字 fold

...学习到此：http://learnyoua.haskell.sg/content/zh-cn/ch06/high-order-function.html