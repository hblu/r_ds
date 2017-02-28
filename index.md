---
title       : R中的数据类型
subtitle    : 
author      : 陆海波
job         : 上海应用技术大学
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [quiz, bootstrap, shiny, interactive, mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
ext_widgets: {rCharts: [libraries/nvd3]}
github:
  user: hblu
  repo: stat
--- &twocol



## 热门电影数据集简介

### 以一个简单的电影票房实际数据为例, 介绍在实际数据处理中R语言中的基本类型、基本操作

*** =left

收集了2016年1月到5月间上映的19部热门电影共10个变量的基本信息，以此为例说明如何在 `R` 语言中进行相关操作

| 变量名        | 说明          | 变量名  | 说明   |
| ------------- |---------------|---------|--------|
| boxoffice     | 上映三月内票房|**director** |导演名字|
| doubanscore   | 豆瓣评分      |**star1**    |主演1   |
| type          | 影片类型      |**index1**   |主演1最近一月的综合搜索指数|
|duration       | 电影时长      |**star2**    |主演2  |
|showtime       |上映日期       |**index2**   |主演2最近一月的综合搜索指数|


*** =right

<img src="assets/img/gongfu.png" class="fit100" />

--- 

## 读入数据

- 当我们把这个数据集第一次读入 `R` 中时, 它会以数据框 `data.frame` 的形式存储
- 暂且可以理解为类似于Excel里常见的表格


```r
movie = read.csv("movie.csv")
xtable(head(movie,3))
```

<!-- html table generated in R 3.3.2 by xtable 1.8-2 package -->
<!-- Wed Mar  1 01:28:49 2017 -->
<table border=1>
<tr> <th>  </th> <th> name </th> <th> boxoffice </th> <th> doubanscore </th> <th> type </th> <th> duration </th> <th> showtime </th> <th> director </th> <th> star1 </th> <th> index1 </th> <th> star2 </th> <th> index2 </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> 叶问3 </td> <td align="right"> 77060.44 </td> <td align="right"> 6.40 </td> <td> 动作 </td> <td align="right"> 105 </td> <td> 2016/3/4 </td> <td> 叶伟信 </td> <td> 甄子丹 </td> <td align="right"> 11385 </td> <td> 张晋 </td> <td align="right"> 4105 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> 美人鱼 </td> <td align="right"> 338583.26 </td> <td align="right"> 6.90 </td> <td> 喜剧 </td> <td align="right">  93 </td> <td> 2016/2/8 </td> <td> 周星驰 </td> <td> 邓超 </td> <td align="right"> 41310 </td> <td> 林允 </td> <td align="right"> 9292 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> 女汉子真爱公式 </td> <td align="right"> 6184.45 </td> <td align="right"> 4.50 </td> <td> 喜剧 </td> <td align="right">  93 </td> <td> 2016/3/18 </td> <td> 郭大雷 </td> <td> 赵丽颖 </td> <td align="right"> 181979 </td> <td> 张翰 </td> <td align="right"> 44277 </td> </tr>
   </table>

---

## 1. 数值型 (`numeric`)

- 比如数据集中的 `doubanscore`, `boxoffice` 
- 通常用 `<-` 或 `=` 给一个变量赋予数字时, 就默认生成数值型数据


```r
x = 2
class(x)
```

```
[1] "numeric"
```

```r
class(movie$boxoffice)
```

```
[1] "numeric"
```

```r
class(movie$doubanscore)
```

```
[1] "numeric"
```

--- &multitext

### 但要注意, 有时候他们会悄悄的出状况

比如你觉得下面几个命了会输出什么结果呢？

1. `exp(1000)`

2. `10/0`

3. `exp(1000)/exp(1000)`

*** .explanation

1. <span class='answer'>Inf</span>
2. <span class='answer'>Inf</span>
3. <span class='answer'>NaN</span>

*** .hint

R会把所有超过电脑存储限制的数字当做正无穷, 一般这个值大约为 $1.8\times 10^{38}$


--- &twocol

## 2. 字符型 (`character`)


*** =left

### 用单引号或双引号定义的就是字符型


```r
x = "2"
class(x)
```

```
[1] "character"
```

```r
y = 'star'
class(y)
```

```
[1] "character"
```

*** =right

- 但并不是所有的文字都是字符型
- 如数据集中的 `director`, `star1`


```r
class(movie$director)
```

```
[1] "factor"
```

```r
class(movie$star1)
```

```
[1] "factor"
```

---

## 3. 逻辑型 (`logical`)

- 逻辑型的结果可以进行加减运算
    - `TRUE` 对应 1
    - `FALSE` 对应 0
- 想知道<美人鱼>是不是一部喜剧?


```r
movie$type[movie$name == "美人鱼"] == "喜剧"
```

```
[1] TRUE
```

- 想在数据集中挑选大于7分的喜剧电影名?


```r
movie$name[movie$type == "喜剧" & movie$doubanscore > 7]
```

```
[1] 功夫熊猫3
19 Levels: 澳门风云三 百鸟朝凤 北京遇上西雅图之不二情书 冰河追凶 大唐玄奘 钢刀 ... 夜孔雀
```

--- &twocol

## 4. 因子型 (`factor`)

*** =left

- 统计中的分类型数据 (定性数据)
- 常用 `factor()` 定义


```r
genders = c("男", "女", "男", "男", "女")
factor(genders)
```

```
[1] 男 女 男 男 女
Levels: 男 女
```

*** =right

字符型数据转换为因子型分成两步

1. 建立字符 `"男"`, `"女"` 与整数 `1`, `2` 的映射关系
  - `"男"` -> 1
  - `"女"` -> 2
2. 按照映射关系, 将 `genders` 转换为整数存储
  - `c(1,2,1,1,2)`


---

### 在 `factor` 中添加参数 `ordered`, 即可存储定序变量


```r
class = factor(c("Poor", "Improved", "Excellent"), ordered = TRUE)
class
```

```
[1] Poor      Improved  Excellent
Levels: Excellent < Improved < Poor
```

- 此时它内部存储方式为 `1=Excellent`, `2=Improved`, `3=Poor`
- 默认按照字母顺序存储
- 可通过增加参数 `levels` 自定义逻辑顺序


```r
class = factor(c("Poor", "Improved", "Excellent"), ordered = TRUE,
               levels = c("Poor", "Improved", "Excellent"))
class
```

```
[1] Poor      Improved  Excellent
Levels: Poor < Improved < Excellent
```


---

## 不同类型数据间如何进行转换?

- 当我们读取一个数据表格时, 如果不做任何处理, 软件会自动把字符型变成因子型变量
- "因子" 实际对应的是定性和定序变量, 因此如果你需要这两组类型的变量出现, 就可以考虑把字符变成因子类型了

### 如何实现字符型和因子型数据的自由切换?

- `as.`函数
    - `as.factor` 把其他类型数据转换成因子型
    - `as.character` 把其他类型数据转换成字符型

---

## 5. 时间类数据 (`Date` / `POSIXct` / `POSIXt`)

- 通常, 时间类数据是以字符串形式输入到 `R` 中的, 因此第一步我们首先需要把这些字符转换成 `R` 可以识别的时间类型数据
    - `Date` 日期数据不包括时间和时区
    - `POSIXct` / `POSIXt` 类型数据包括日期, 时间和时区

---

## 如何把字符转化成 `Date` 日期格式?

- 用 `as.Date` 函数转换
- 通过参数 `format` 指定输入字符的格式




```r
class(movie$showtime)
```

```
[1] "factor"
```

```r
movie$showtime <- as.Date(movie$showtime)
head(movie$showtime)
```

```
[1] "2016-03-04" "2016-02-08" "2016-03-18" "2016-02-08" "2016-02-08" "2016-01-29"
```

---


### `as.Date` 所能接受的默认时间格式为 `2017-03-01` 或 `2017/03/01`

如果不是这样的格式怎么办?

>- 通过添加参数 `format`


```r
x <- c("1jan1960", "2jan1960", "31mar1960", "30jul1960")
y <- as.Date(x)
```

```
Error in charToDate(x): character string is not in a standard unambiguous format
```

```r
y <- as.Date(x,format="%d%b%Y")
y
```

```
[1] "1960-01-01" "1960-01-02" "1960-03-31" "1960-07-30"
```

---

## 日期格式表

| 符号        | 含义         | 示例     |
| ------------- |---------------|---------|
|   `%d`   |  days as a number (1-31)    |  01-31     |
|   `%a`   |  abbreviated weekday    |  Mon     |
|   `%A`   |  unabbreviated weekday     |  Monday     |
|   `%m`   |  month (01-12)    |  01-12     |
|   `%b`   |  abbreviated month    |  Jan     |
|   `%B`   |  abbreviated month    |  January     |
|   `%y`   |  2-digit year    |  17     |
|   `%Y`   |  4-digit year    |  2017     |

---

## 如何把字符转化成 `POSIXct` / `POSIXt` 时间格式

- 函数 `as.POSIXct`
- 默认格式是 `2017/2/27  10:15:24`, `2017-2-27    10:15:24`
    

```r
as.POSIXct("March-03-2017 01:30:00")
```

```
Error in as.POSIXlt.character(x, tz, ...): character string is not in a standard unambiguous format
```

```r
as.POSIXct("March-03-2017 01:30:00",format="%B-%d-%Y %H:%M:%S")
```

```
[1] "2017-03-03 01:30:00 CST"
```

--- 

## 如何把时间摆弄成你想要的形式?

函数 `format` 可以用来更改时间数据的输出格式, 甚至还可以提取你想要的某个部分




```r
m=head(movie$showtime, 1) #原始日期数据
format(m,format="%B %d %Y") #改成月日年的格式
```

```
[1] "March 04 2016"
```

```r
format(m,format="%B %d %Y %A") #加入星期信息
```

```
[1] "March 04 2016 Friday"
```

```r
format(m,format="%B") #只提取出月份信息
```

```
[1] "March"
```

---


```r
Sys.time()#输出系统时间
```

```
[1] "2017-03-01 01:28:50 CST"
```

```r
class(Sys.time())#查看时间类型
```

```
[1] "POSIXct" "POSIXt" 
```

```r
format(Sys.time(),format = "%B %d %Y")#提取部分时间信息
```

```
[1] "March 01 2017"
```

```r
format(Sys.time(),format = "%Y/%B/%a %H:%M:%S")#提取部分时间信息
```

```
[1] "2017/March/Wed 01:28:50"
```

---

## `lubridate`: 一款处理时间数据的专用包

主要有两类函数

- 处理时点数据
- 处理时段数据


```r
library(lubridate)
x <- c(20090101,"2009-01-02","2009 01 03","2009-1-4","2009-1,5","Created on 2009 1 6",
       "200901 !!! 07")
x = ymd(x)
x
```

```
[1] "2009-01-01" "2009-01-02" "2009-01-03" "2009-01-04" "2009-01-05" "2009-01-06" "2009-01-07"
```

---


```r
mday(x)
```

```
[1] 1 2 3 4 5 6 7
```

```r
wday(x)
```

```
[1] 5 6 7 1 2 3 4
```

```r
hour("2015-11-20 01:30:00")
```

```
[1] 1
```

```r
minute("2015-11-20 01:30:00")
```

```
[1] 30
```

---

## 时间类数据的操作

- 求任意两个日期距离的天数


```r
begin=as.Date("2016-03-04")
end=as.Date("2016-05-08")
(during=end-begin)
```

```
Time difference of 65 days
```

- 求任意两个日期距离的周数和小时数


```r
difftime(end,begin,units = "weeks")
```

```
Time difference of 9.29 weeks
```

```r
difftime(end,begin,units = "hours")
```

```
Time difference of 1560 hours
```

---

## 排序

- 单独对时间进行排序


```r
head(movie$showtime)
```

```
[1] "2016-03-04" "2016-02-08" "2016-03-18" "2016-02-08" "2016-02-08" "2016-01-29"
```

```r
head(sort(movie$showtime))
```

```
[1] "2016-01-29" "2016-02-08" "2016-02-08" "2016-02-08" "2016-03-04" "2016-03-18"
```

---

- 对数据表格中的数据按照时间顺序排列


```r
res = head(movie[order(movie$showtime),c("name","showtime")])
kable(res, type = "html")
```



|   |name                     |showtime   |
|:--|:------------------------|:----------|
|6  |功夫熊猫3                |2016-01-29 |
|2  |美人鱼                   |2016-02-08 |
|4  |西游记之孙悟空三打白骨精 |2016-02-08 |
|5  |澳门风云三               |2016-02-08 |
|1  |叶问3                    |2016-03-04 |
|3  |女汉子真爱公式           |2016-03-18 |
