# GO 学习笔记

# 学习目标

* 能够快速掌握Go基本语法
* 能够修改、调试Go的项目代码
* 对目前主流框架的情况有所了解及选型
* 能够创建API服务、分布式服务等
* 能够快速部署Go应用程序


# 基础语法

## 命名

### 25个关键字

```
break      default       func     interface   select
case       defer         go       map         struct
chan       else          goto     package     switch
const      fallthrough   if       range       type
continue   for           import   return      var
```

### 30+个预定义名字

```
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error

内建函数: make len cap new append copy close delete
          complex real imag
          panic recove
```

## 变量声明

### 语法

```
var 变量名字 类型 = 表达式
var i, j, k int                 // int, int, int
var b, f, s = true, 2.3, "four" // bool, float64, string
var f, err = os.Open(name)
```

### 简短声明
> 变量的类型根据表达式来自动推导, 广泛用于大部分的局部变量的声明和初始化

```
anim := gif.GIF{LoopCount: nframes}
freq := rand.Float64() * 3.0
t := 0.0
i, j := 0, 1
```

## 指针

> 那么&x表达式（取x变量的内存地址）, 指针对应的数据类型是*int，指针被称之为“指向int类型的指针”

* 任何类型的指针的零值都是nil
* 表达式也必须能接受&取地址操作
* 指针之间也是可以进行相等测试的，只有当它们指向同一个变量或全部是nil时才相等
* 编译器会自动选择在栈上还是在堆上分配局部变量的存储空间
* new(Type) = &Type{}

```
x := 1
p := &x         // p 为 *int 指向 x 的内存地址
fmt.Println(*p) // "1"
*p = 2          // 更改p指向的内存地址的值 to x = 2
fmt.Println(x)  // "2"

var p = f()

func f() *int {
    v := 1
    return &v
}

// 每次调用f函数都将返回不同的结果：

fmt.Println(f() == f()) // "false"
```
通过指针来更新变量的值，然后返回更新后的值

```
func incr(p *int) int {
    *p++ // 非常重要：只是增加p指向的变量的值，并不改变p指针！！！
    return *p
}

v := 1
incr(&v)              // side effect: v is now 2
fmt.Println(incr(&v)) // "3" (and v is 3)
```

## 类型

> type 类型名字 底层类型

```
type Celsius float64    // 摄氏温度
type Fahrenheit float64 // 华氏温度

const (
    AbsoluteZeroC Celsius = -273.15 // 绝对零度
    FreezingC     Celsius = 0       // 结冰点温度
    BoilingC      Celsius = 100     // 沸水温度
)

func CToF(c Celsius) Fahrenheit { return Fahrenheit(c*9/5 + 32) }

func FToC(f Fahrenheit) Celsius { return Celsius((f - 32) * 5 / 9) }
```


## 数据类型

### 整数

```
var u uint8 = 255
var i int8 = 127

```
### 浮点数
```
var f float32 = 16777216 // 1 << 24
const e = 2.71828 // (approximately)
const Avogadro = 6.02214129e23  // 阿伏伽德罗常数
const Planck   = 6.62606957e-34 // 普朗克常数
```
### 复数
```
var x complex128 = complex(1, 2) // 1+2i
var y complex128 = complex(3, 4) // 3+4i
```

### 布尔型

```
func btoi(b bool) int {
    if b {
        return 1
    }
    return 0
}
```

### 字符串

```
s := "hello, world"
fmt.Println(len(s))     // "12"
fmt.Println(s[0], s[7]) // "104 119" ('h' and 'w')
fmt.Println(s[:5]) // "hello"
fmt.Println(s[7:]) // "world"
fmt.Println(s[:])  // "hello, world"
n := 0
for _, _ = range s {
    n++
}

// 字符串和Byte切片
// 标准库中有四个包对字符串处理尤为重要：bytes、strings、strconv和unicode包。strings包提供了许多如字符串的查询、替换、比较、截断、拆分和合并等功能。

```
### 常量
> 和变量声明一样，可以批量声明多个常量

> 常量声明可以使用iota常量生成器初始化，它用于生成一组以相似规则初始化的常量，但是不用每行都写一遍初始化表达式。在一个const声明语句中，在第一个声明的常量所在的行，iota将会被置为0，然后在每一个有常量声明的行加一。

```
const (
    e  = 2.71828182845904523536028747135266249775724709369995957496696763
    pi = 3.14159265358979323846264338327950288419716939937510582097494459
)


const (
    a = 1
    b
    c = 2
    d
)


type Weekday int

const (
    Sunday Weekday = iota
    Monday
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
)
```

##  复合数据类型

### 数组

```
var a [3]int   
var q [3]int = [3]int{1, 2, 3}
var r [3]int = [3]int{1, 2}
q := [...]int{1, 2, 3}
q := [3]int{1, 2, 3}
q = [4]int{1, 2, 3, 4}

a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}

fmt.Println(a == b, a == c, b == c) // "true false false"
d := [3]int{1, 2}
fmt.Println(a == d) // compile error: cannot compare [2]int == [3]int

// 定义了一个含有100个元素的数组r，最后一个元素被初始化为-1，其它元素都是用0初始化。
r := [...]int{99: -1}

// print
for i, v := range a {
    fmt.Printf("%d %d\n", i, v)
}

// 打印金钱单位
type Currency int

const (
    USD Currency = iota // 美元
    EUR                 // 欧元
    GBP                 // 英镑
    RMB                 // 人民币
)

symbol := [...]string{USD: "$", EUR: "€", GBP: "￡", RMB: "￥"}

fmt.Println(RMB, symbol[RMB]) // "3 ￥"

```

### slice(切片)
> 切片是一个引用类型，对数组一个连续片段的引用

* 由 len() 函数获取长度
* cap() 可以测量切片最长可以达到多少：它等于切片的长度 + 数组除切片之外的长度，切片 s 来说该不等式永远成立：0 <= len(s) <= cap(s)。
* 多个切片如果表示同一个数组的片段，它们可以共享数

> 切片初始化

```
var identifier []type（不需要说明长度）
var slice1 []type = arr1[start:end]
// 对于每一个切片（包括 string），以下状态总是成立的 
s == s[:i] + s[i:] // i是一个整数且: 0 <= i <= len(s)
len(s) < cap(s)
```

> make() 创建一个切片
> make 接受 2 个参数：元素的类型以及切片的元素个数。

```
var slice1 []type = make([]type, len)。
// 简写
slice1 := make([]type, len)
// 如果你想创建一个 slice1，它不占用整个数组，而只是占用以 len 为个数个项，
slice1 := make([]type, len, cap)。
// make 的使用方式是：
func make([]T, len, cap)，其中 cap 是可选参数。
```
> 下面两种方法可以生成相同的切片

```
make([]int, 50, 100)
new([100]int)[0:50]
```

* new(T) 为每个新的类型T分配一片内存，初始化为 0 并且返回类型为*T的内存地址：这种方法 返回一个指向类型为 T，值为 0 的地址的指针，它适用于值类型如数组和结构体（参见第 10 章）；它相当于 &T{}
* make(T) 返回一个类型为 T 的初始值，它只适用于3种内建的引用类型：切片、map 和 channel


#### 切片的追加与复制

```
func main() {
    sl_from := []int{1, 2, 3}
    sl_to := make([]int, 10)

    n := copy(sl_to, sl_from)  // 复制
    fmt.Println(sl_to)
    fmt.Printf("Copied %d elements\n", n) // n == 3

    sl3 := []int{1, 2, 3}
    sl3 = append(sl3, 4, 5, 6)
    fmt.Println(sl3)
}
```
> 如果 s 的容量不足以存储新增元素，append 会分配新的切片来保证已有切片元素和新增元素的存储
> 如果你想将切片 y 追加到切片 x 后面，只要将第二个参数扩展成一个列表即可：x = append(x, y...)
 

```
func append(s[]T, x ...T) []T
```

### Map

> Map的创建方式如下：

```
// 语法
make(map[keyType]valueType, initialCapacity)
make(map[keyType]valueType)
map[keyType]valueType{}
map[keyType]valueType{key1: value1, key2: value2, ..., keyN: valueN}

// 示例
	var mapLit map[string]int
	var mapCreated map[string]float32
	var mapAssigned map[string]int

    mapLit = map[string]int{"one": 1, "two": 2}
    mapCreated := make(map[string]float32)
    mapAssigned = mapLit

    mapCreated["key1"] = 4.5
    mapCreated["key2"] = 3.14159
    mapAssigned["two"] = 3

// 判断是否存在
_, ok := map1[key1] // 如果key1存在则ok == true，否在ok为false
// 混合使用
if _, ok := map1[key1]; ok {
    // ...
}
// 删除
delete(map1, key1）
// for-range 用法
for key, value := range map1 {
    ...
}
// 切片类型
// Version A:
    items := make([]map[int]int, 5)
    for i:= range items {
        items[i] = make(map[int]int, 1)
        items[i][1] = 2
    }
    fmt.Printf("Version A: Value of items: %v\n", items)

    // Version B: NOT GOOD!
    items2 := make([]map[int]int, 5)
    for _, item := range items2 {
        item = make(map[int]int, 1) // item is only a copy of the slice element.
        item[1] = 2 // This 'item' will be lost on the next iteration.
    }
    fmt.Printf("Version B: Value of items: %v\n", items2)
```

## 测试工具

### wrk 压力测试工具

#### 参考文章

- http://blog.csdn.net/alexander_wang_it/article/details/52214558
- http://zjumty.iteye.com/blog/2221040

```
// 用12个线程模拟100个连接持续30s
wrk -t12 -c100 -d30s  http://www.163.com
```
> 参数描述

- `-t` 模拟线程数
- `-c` 模拟连接数
- `-T` 模拟超时
- `-d` 持续测试时间
- `--latency` 响应时间分布
- `--script` 加载`lua`脚本
	- 创建 `post.lua`

```
wrk.method = "POST"
wrk.body = "foo=bar&baz=quux"
wrk.headers["Content-Type"] = "application/x-www-form-urlencoded"
// 更多属性
local wrk = {
	scheme = "http",
	host = "localhost",
	port = nil,
	method = "GET",
	path = "/",
	headers = {},
	body = nil,
    thread = nil
}
```

```
// 执行
wrk -t12 -c100 -d30s -T30s --script=post.lua --latency http://www.baidu.com
```

#### Function
```
local counter = 1  
local threads = {}  

function setup(thread)  
   thread:set("id", counter)  
   table.insert(threads, thread)  
   counter = counter + 1  
end  

function init(args)  
   requests  = 0  
   responses = 0  

   local msg = "thread %d created"  
   print(msg:format(id))  
end  

function request()  
   requests = requests + 1  
   return wrk.request()  
end  

function response(status, headers, body)  
   responses = responses + 1  
end  

function done(summary, latency, requests)  
   for index, thread in ipairs(threads) do  
      local id        = thread:get("id")  
      local requests  = thread:get("requests")  
      local responses = thread:get("responses")  
      local msg = "thread %d made %d requests and got %d responses"  
      print(msg:format(id, requests, responses))  
   end  
end  
```
#### Instance
#### bash
```
#!/bin/bash

head="threadPool,concurrent,latency.min,latency.max,latency.mean,latency.stdev,upper_50,upper_90,upper_95,upper_99,duration,requests,bytes,errors.connect,errors.read,errors.write,errors.status,errors.timeout,qps,Transfer/sec(KB)"

## threadPoolSize
threadPool=100
## 压测持续时间，单位s、m
duration=1s
## 每轮压测结束后休息时间
sleepTime=5s
## 压测路径URL
url="http://localhost:38290/rpc" 

## 间隔递增并发数
concurrentInterval=12
## 停止压测并发数,超过此值停止压测
stopConcurrent=1000
# 并发计数器
i=0

echo $head;
while (($i <= $stopConcurrent))
do
    let i+=concurrentInterval
    ret=`wrk -t12 -c$i -d$duration --latency -s post.lua $url | sed -n '$p'`
    ret="$threadPool,$i,"$ret;
    echo $ret;
    sleep $sleepTime
done
```
#### post.lua

```
function response(status, headers, body)
  --print(body)
end

function init(args)
    for k, v in pairs(args) do
        --print(k, v)
    end
end

function done(summary, latency, requests)
  print("latency.min,latency.max,latency.mean,latency.stdev,latency:percentile(50.0),latency:percentile(90.0),latency:percentile(95.0),latency:percentile(99.0),summary.duration,summary.requests,summary.bytes,summary.errors.connect,summary.errors.read,summary.errors.write,summary.errors.status,summary.errors.timeout,qps,Transfer/sec(KB)")
  print(latency.min/1000 .. "," .. latency.max/1000 .. "," .. latency.mean/1000 .. "," .. latency.stdev/1000 .. "," .. latency:percentile(50.0)/1000 .. "," .. latency:percentile(90.0)/1000 .. "," .. latency:percentile(95.0)/1000 .. "," .. latency:percentile(99.0)/1000 .. "," .. summary.duration/1000000 .. "," .. summary.requests .. "," .. summary.bytes .. "," .. summary.errors.connect.. "," .. summary.errors.read.. "," .. summary.errors.write.. "," .. summary.errors.status.. "," .. summary.errors.timeout .. "," .. summary.requests/(summary.duration/1000000) .. "," .. (summary.bytes/1024)/(summary.duration/1000000))
end

function file_exists(file)
  local f = io.open(file, "rb")
  if f then f:close() end
  return f ~= nil
end

function shuffle(paths)
  local j, k
  local n = #paths
  for i = 1, n do
    j, k = math.random(n), math.random(n)
    paths[j], paths[k] = paths[k], paths[j]
  end
  return paths
end

function non_empty_lines_from(file)
  if not file_exists(file) then return {} end
  lines = {}
  for line in io.lines(file) do
    if not (line == '') then
      lines[#lines + 1] = line
    end
  end
  return shuffle(lines)
end



function request()
    path = paths[counter]
    counter = counter + 1
    if counter > #paths then
      counter = 1
    end

    wrk.method = "POST"
    wrk.body = path
    wrk.headers["Content-Type"] = "application/json;charset=utf-8"
    return wrk.format(wrk.method,nil,wrk.headers,wrk.body)
end

counter = 1
math.randomseed(os.time())
math.random(); math.random(); math.random()

paths = non_empty_lines_from("paths.txt")

if #paths <= 0 then
  print("multiplepaths: No paths found. You have to create a file paths.txt with one path per line")
  os.exit()
end
print("multiplepaths: Found " .. #paths .. " paths")
```
#### paths.txt

```
{"ver":"1.0","soa":{"rpc":"1.2649","req":"86eca521-34ce-4f95-b114-ae966e2e04bc"},"iface":"me.ele.napos.order.proxy.api.TestService","method":"test","args":{},"metas":{}}
{"ver":"1.0","soa":{"rpc":"1.2649","req":"86eca521-34ce-4f95-b114-ae966e2e04bc"},"iface":"me.ele.napos.order.proxy.api.QueryOrderService","method":"getUnprocessedOrders","args":{"restaurantId":"59968"},"metas":{}}
```

### ApacheBench

> ab 压力测试工具

#### 参考资料
- http://www.ha97.com/4617.html

#### ApacheBench参数说明

```
格式：ab [options] [http://]hostname[:port]/path
参数说明：
-n requests Number of requests to perform
//在测试会话中所执行的请求个数（本次测试总共要访问页面的次数）。默认时，仅执行一个请求。
-c concurrency Number of multiple requests to make
//一次产生的请求个数（并发数）。默认是一次一个。
-t timelimit Seconds to max. wait for responses
//测试所进行的最大秒数。其内部隐含值是-n 50000。它可以使对服务器的测试限制在一个固定的总时间以内。默认时，没有时间限制。
-p postfile File containing data to POST
//包含了需要POST的数据的文件，文件格式如“p1=1&p2=2”.使用方法是 -p 111.txt 。 （配合-T）
-T content-type Content-type header for POSTing
//POST数据所使用的Content-type头信息，如 -T “application/x-www-form-urlencoded” 。 （配合-p）
-v verbosity How much troubleshooting info to print
//设置显示信息的详细程度 – 4或更大值会显示头信息， 3或更大值可以显示响应代码(404, 200等), 2或更大值可以显示警告和其他信息。 -V 显示版本号并退出。
-w Print out results in HTML tables
//以HTML表的格式输出结果。默认时，它是白色背景的两列宽度的一张表。
-i Use HEAD instead of GET
// 执行HEAD请求，而不是GET。
-x attributes String to insert as table attributes
-y attributes String to insert as tr attributes
-z attributes String to insert as td or th attributes
-C attribute Add cookie, eg. -C “c1=1234,c2=2,c3=3” (repeatable)
//-C cookie-name=value 对请求附加一个Cookie:行。 其典型形式是name=value的一个参数对。此参数可以重复，用逗号分割。
提示：可以借助session实现原理传递 JSESSIONID参数， 实现保持会话的功能，如
-C ” c1=1234,c2=2,c3=3, JSESSIONID=FF056CD16DA9D71CB131C1D56F0319F8″ 。
-H attribute Add Arbitrary header line, eg. ‘Accept-Encoding: gzip’ Inserted after all normal header lines. (repeatable)
-A attribute Add Basic WWW Authentication, the attributes
are a colon separated username and password.
-P attribute Add Basic Proxy Authentication, the attributes
are a colon separated username and password.
//-P proxy-auth-username:password 对一个中转代理提供BASIC认证信任。用户名和密码由一个:隔开，并以base64编码形式发送。无论服务器是否需要(即, 是否发送了401认证需求代码)，此字符串都会被发送。
-X proxy:port Proxyserver and port number to use
-V Print version number and exit
-k Use HTTP KeepAlive feature
-d Do not show percentiles served table.
-S Do not show confidence estimators and warnings.
-g filename Output collected data to gnuplot format file.
-e filename Output CSV file with percentages served
-h Display usage information (this message)
//-attributes 设置属性的字符串. 缺陷程序中有各种静态声明的固定长度的缓冲区。另外，对命令行参数、服务器的响应头和其他外部输入的解析也很简单，这可能会有不良后果。它没有完整地实现 HTTP/1.x; 仅接受某些’预想’的响应格式。 strstr(3)的频繁使用可能会带来性能问题，即你可能是在测试ab而不是服务器的性能。

参数很多，一般我们用 -c 和 -n 参数就可以了。例如:

# 网址最后一个必须是/
# ab -c 5000 -n 600 http://www.163.com/
```

#### 结果分析

```
Document Path: /phpinfo.php
#测试的页面
Document Length: 50797 bytes
#页面大小

Concurrency Level: 1000
#测试的并发数
Time taken for tests: 11.846 seconds
#整个测试持续的时间
Complete requests: 4000
#完成的请求数量
Failed requests: 0
#失败的请求数量
Write errors: 0
Total transferred: 204586997 bytes
#整个过程中的网络传输量
HTML transferred: 203479961 bytes
#整个过程中的HTML内容传输量
Requests per second: 337.67 [#/sec] (mean)
#最重要的指标之一，相当于LR中的每秒事务数，后面括号中的mean表示这是一个平均值
Time per request: 2961.449 [ms] (mean)
#最重要的指标之二，相当于LR中的平均事务响应时间，后面括号中的mean表示这是一个平均值
Time per request: 2.961 [ms] (mean, across all concurrent requests)
#每个连接请求实际运行时间的平均值
Transfer rate: 16866.07 [Kbytes/sec] received
#平均每秒网络上的流量，可以帮助排除是否存在网络流量过大导致响应时间延长的问题
Connection Times (ms)
min mean[+/-sd] median max
Connect: 0 483 1773.5 11 9052
Processing: 2 556 1459.1 255 11763
Waiting: 1 515 1459.8 220 11756
Total: 139 1039 2296.6 275 11843
#网络上消耗的时间的分解，各项数据的具体算法还不是很清楚

Percentage of the requests served within a certain time (ms)
50% 275
66% 298
75% 328
80% 373
90% 3260
95% 9075
98% 9267
99% 11713
100% 11843 (longest request)
#整个场景中所有请求的响应情况。在场景中每个请求都有一个响应时间，其中50％的用户响应时间小于275毫秒，66％的用户响应时间小于298毫秒，最大的响应时间小于11843毫秒。对于并发请求，cpu实际上并不是同时处理的，而是按照每个请求获得的时间片逐个轮转处理的，所以基本上第一个Time per request时间约等于第二个Time per request时间乘以并发请求数。
```

# 包管理工具

## Glide

- `glide create / init`: 初始化一个新的项目，创建一个`glide.yml`
- `glide get`: 安装一个或者更多的包到`vendor/`，并添加依赖到 `glide.yml`
- `glide remove / rm `

> 参考资料：https://blog.wu-boy.com/2016/05/package-management-for-golang-glide/

# 参考资料

* 学习GO语言：https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/directory.md
* GO语言圣经：https://docs.ruanjiadeng.com/gopl-zh/index.html