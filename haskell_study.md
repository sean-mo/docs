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

