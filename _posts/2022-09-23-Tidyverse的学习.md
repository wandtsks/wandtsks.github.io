```shell
layout: post
title: "POST TITLE"
date: 2022-9-23 hh:mm:ss -0000
categories: CATEGORY-1
```

# Tidyverse的学习

## 1.管道函数（%>%）
**实现高速运算的关键**
```
a - < (n = 100 , mean = 0 , s = 15) %>% round() %>% abs() %>% sort() %>% diff()
#其中，round() %>% abs() %>% sort() %>% diff()
#用管道函数可以避免太多函数一起计算时套娃的现象，不容易混淆
```
## 2 数据清洗助手 group_by（）
提供根据指定变量分组的功能
返回值为分组后的数据
对每组数据进行之后的操作
可以看成是一个**循坏对不同组数据处理**的简便功能
## 3. 数据筛选 select()
从数据中筛选出 某些行 和 某些列

```
> head(iris)
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
> 
> iris %>% select (Sepal.Length) %>% head()
  Sepal.Length
1          5.1
2          4.9
3          4.7
4          4.6
5          5.0
6          5.4
```
如果要用变量中的名字作为删选条件，可以用 stats_with
```
 iris %>% select(starts_with('Petal')) %>% head()
  Petal.Length Petal.Width
1          1.4         0.2
2          1.4         0.2
3          1.3         0.2
4          1.5         0.2
5          1.4         0.2
6          1.7         0.4
```
## 4.新增变量 mutate()
首先提取汽车数据中的 mpg wt
```
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
> a <- mtcars %>% select(c(mpg,wt))
> a
                     mpg    wt
Mazda RX4           21.0 2.620
Mazda RX4 Wag       21.0 2.875
Datsun 710          22.8 2.320
Hornet 4 Drive      21.4 3.215
Hornet Sportabout   18.7 3.440
Valiant             18.1 3.460
Duster 360          14.3 3.570
Merc 240D           24.4 3.190
Merc 230            22.8 3.150
Merc 280            19.2 3.440
Merc 280C           17.8 3.440
Merc 450SE          16.4 4.070
Merc 450SL          17.3 3.730
Merc 450SLC         15.2 3.780
Cadillac Fleetwood  10.4 5.250
Lincoln Continental 10.4 5.424
Chrysler Imperial   14.7 5.345
Fiat 128            32.4 2.200
Honda Civic         30.4 1.615
Toyota Corolla      33.9 1.835
Toyota Corona       21.5 2.465
Dodge Challenger    15.5 3.520
AMC Javelin         15.2 3.435
Camaro Z28          13.3 3.840
Pontiac Firebird    19.2 3.845
Fiat X1-9           27.3 1.935
Porsche 914-2       26.0 2.140
Lotus Europa        30.4 1.513
Ford Pantera L      15.8 3.170
Ferrari Dino        19.7 2.770
Maserati Bora       15.0 3.570
Volvo 142E          21.4 2.780
```
然后进行分组
```
> a <- as_tibble(a)
> a
# A tibble: 32 × 2
     mpg    wt
   <dbl> <dbl>
 1  21    2.62
 2  21    2.88
 3  22.8  2.32
 4  21.4  3.22
 5  18.7  3.44
 6  18.1  3.46
 7  14.3  3.57
 8  24.4  3.19
 9  22.8  3.15
10  19.2  3.44
# … with 22 more rows
# ℹ Use `print(n = ...)` to see more rows
```
使用mutate 进行新增变量 wt1
```
> a <- a %>% mutate(wt1 = wt-1)
> a
# A tibble: 32 × 3
     mpg    wt   wt1
   <dbl> <dbl> <dbl>
 1  21    2.62  1.62
 2  21    2.88  1.88
 3  22.8  2.32  1.32
 4  21.4  3.22  2.22
 5  18.7  3.44  2.44
 6  18.1  3.46  2.46
 7  14.3  3.57  2.57
 8  24.4  3.19  2.19
 9  22.8  3.15  2.15
10  19.2  3.44  2.44
# … with 22 more rows
# ℹ Use `print(n = ...)` to see more rows
```
或者进行其他运算
```
> a <- a %>% mutate(mpgz = scale(mpg))
> a
# A tibble: 32 × 4
     mpg    wt   wt1 mpgz[,1]
   <dbl> <dbl> <dbl>    <dbl>
 1  21    2.62  1.62    0.151
 2  21    2.88  1.88    0.151
 3  22.8  2.32  1.32    0.450
 4  21.4  3.22  2.22    0.217
 5  18.7  3.44  2.44   -0.231
 6  18.1  3.46  2.46   -0.330
 7  14.3  3.57  2.57   -0.961
 8  24.4  3.19  2.19    0.715
 9  22.8  3.15  2.15    0.450
10  19.2  3.44  2.44   -0.148
# … with 22 more rows
# ℹ Use `print(n = ...)` to see more rows
```
## 5. 变量排序 arrange()

还是以mtcars 为例

提取三个变量数据 mpg,am,gear

```R
> b %>% arrange(mpg)
                     mpg am gear
Cadillac Fleetwood  10.4  0    3
Lincoln Continental 10.4  0    3
Camaro Z28          13.3  0    3
Duster 360          14.3  0    3
Chrysler Imperial   14.7  0    3
Maserati Bora       15.0  1    5
Merc 450SLC         15.2  0    3
AMC Javelin         15.2  0    3
Dodge Challenger    15.5  0    3
Ford Pantera L      15.8  1    5
Merc 450SE          16.4  0    3
Merc 450SL          17.3  0    3
Merc 280C           17.8  0    4
Valiant             18.1  0    3
Hornet Sportabout   18.7  0    3
Merc 280            19.2  0    4
Pontiac Firebird    19.2  0    3
Ferrari Dino        19.7  1    5
Mazda RX4           21.0  1    4
Mazda RX4 Wag       21.0  1    4
Hornet 4 Drive      21.4  0    3
Volvo 142E          21.4  1    4
Toyota Corona       21.5  0    3
Datsun 710          22.8  1    4
Merc 230            22.8  0    4
Merc 240D           24.4  0    4
Porsche 914-2       26.0  1    5
Fiat X1-9           27.3  1    4
Honda Civic         30.4  1    4
Lotus Europa        30.4  1    5
Fiat 128            32.4  1    4
Toyota Corolla      33.9  1    4

```

如果是两个排序依据，可以写成

> b %>% arrange(am,mpg)

默认是正序，如果是倒叙，则是

> b %>% arrange(-mpg)



## 6.分类汇总 summarise（）

```
> b %>% group_by(am) %>% summarise(meanmpg = mean(mpg),sdmpg = sd(mpg),maxmpg = max(mpg) , minmpg = min(mpg))
# A tibble: 2 × 5
     am meanmpg sdmpg maxmpg minmpg
  <dbl>   <dbl> <dbl>  <dbl>  <dbl>
1     0    17.1  3.83   24.4   10.4
2     1    24.4  6.17   33.9   15 
```



