

## DAX函数

```python
# 带着漏斗的计算器  
CALCULATE([销售数量],'产品表'[产品大类]="办公")

CALCULATE([销售数量],'产品表'[产品大类] = "办公" || '产品表'[产品大类]="技术")
CALCULATE([销售数量],'地区表'[区域]="东北" , '产品表'[产品大类]= "办公")
```



```python
# 求和
sum('订单表'[数量])
```

```python
# 安全除法
DIVIDE([利润金额],[销售金额])
```

```python
# 迭代函数 求平均值
AVERAGE('订单表'[发货天数])
```

```python
# 日期函数，求发货天数
DATEDIFF('订单表'[订单日期],'订单表'[发货日期],DAY)
```

```python
# 逻辑函数
if('订单表'[利润]>0,"盈利","亏损")
```

```python
# 迭代函数 
=SUMX('订单表','订单表'[金额]*(1-'订单表'[折扣]))
```

```python 
# 迭代函数 统计排名序号
COUNTROWS(FILTER('订单表','订单表'[金额]>=EARLIER('订单表'[金额])))
```

```python
# 关联函数     将维度表的字段关联至事实表
RELATED('地区表'[区域])
```

![image-20230302121929047](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/image-20230302121929047.png)

```python
# 关联函数  将事实表的字段关联至维度表  RELATEDTABLE后面必须是一个表，不能是一个字段
SUMX(RELATEDTABLE('订单表'),'订单表'[金额])
```

![image-20230302122134150](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/image-20230302122134150.png)

![image-20230302123923253](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/image-20230302123923253.png)

```python
# 关系函数 统计不同时间口径的指标
CALCULATE('订单表'[订单数],USERELATIONSHIP('订单表'[订单日期],'日历表'[日期]))
```

#### 筛选函数

```python
# 筛选函数 filter，通长搭配calculate
CALCULATE([销售金额],FILTER('地区表','地区表'[区域]="东北"),FILTER('产品表','产品表'[产品大类]="办公"))

CALCULATE([销售金额],FILTER('产品表','产品表'[产品大类]="办公"))

CALCULATE([销售金额],FILTER('产品表','产品表'[产品大类] in {"办公","技术"}))

CALCULATE([销售金额],FILTER('产品表','产品表'[产品大类]="办公" || '产品表'[产品大类]="技术"))

CALCULATE([销售金额],FILTER('产品表','产品表'[产品大类]="技术"))
```

```python
# 筛选条件 all 
CALCULATE([销售金额],ALL('产品表'),ALL('订单表'))
```

```python
# 筛选条件 allexpect
CALCULATE([销售金额],ALLEXCEPT('订单表','订单表'[区域]))
```

```python
# 筛选条件 allselect
```

#### 时间智能函数

| 函数名称                                                     | 语法(==CALCULATE+时间智能函数==)                    | 注释                                        |
| ------------------------------------------------------------ | --------------------------------------------------- | ------------------------------------------- |
| dateadd                                                      | dateadd(日期列, 偏移数量, 偏移单位)                 | 以当前日期开始，按指定数量和单位偏移        |
| parallelperiod                                               | parallelperiod(日期列, 偏移数量, 偏移单位)          | 以当前日期开始，按指定数量和单位偏移(无day) |
| datesinperiod                                                | datesinperiod(日期列, 开始日期, 偏移数量, 偏移单位) | 以指定开始日期开始，按指定数量和单位偏移    |
| datesbetween                                                 | datesbetween(日期列, 开始日期, 结束日期)            | 以指定开始日期和结束日期偏移                |
| ==偏移单位(时间间隔)：year、quarter、month、day==            |                                                     |                                             |
| ==偏移数量：正数则向未来偏移，负数则向过去偏移，零则不偏移== |                                                     |                                             |
| sameperiodlastyear                                           | sameperiodlastyear(日期列)                          | 去年同期, same period last year             |
| nextday                                                      | nextday(日期列)                                     | 下一天,  next day,                          |
| nextmonth                                                    | nextmonth(日期列)                                   | 下个月,  next month,                        |
| nextquarter                                                  | nextquarter(日期列)                                 | 下季度,  next quarter                       |
| nextyear                                                     | nextyear(日期列)                                    | 下一年, next year                           |
| previousday                                                  | previousday(日期列)                                 | 上一天, previous day                        |
| previousmonth                                                | previousmonth(日期列)                               | 上个月, previous month                      |
| previousquarter                                              | previousquarter(日期列)                             | 上季度, previous quarter                    |
| previousyear                                                 | previousyear(日期列)                                | 上一年, previous year                       |
| totalmtd                                                     | totalmtd(表达式, 日期列)                            | 本月至今的累计值, total month to date       |
| totalqtd                                                     | totalqtd(表达式, 日期列)                            | 本季至今的累计值, total quarter to date     |
| totalytd                                                     | totalytd(表达式, 日期列)                            | 本年至今的累计值, total year to date        |
| https://docs.microsoft.com/zh-cn/dax/time-intelligence-functions-dax |                                                     |                                             |

#### VAR和RETURN

```python
VAR sales = SUM('订单表'[数量])  //本月销售数量  
VAR slaes_lm = CALCULATE(SUM('日历表'[日期]),DATEADD('日历表'[日期],-1,MONTH))  //上月的销售数量
RETURN DIVIDE(sales,slaes_lm)-1

```

#### VALUES

```python
VALUES：返回由一列构成的表，且不含重复值

#一般跟它们搭配使用
CALCULATE + FILTER + VALUES

CALCULATE([产品小类数量],FILTER(VALUES('产品表'[产品小类]),[销售数量]>2000))
```

## PowerBI

**DAX公式新建表**

```python
汇总表 = SUMMARIZE('订单表','地区表'[区域],'产品表'[产品大类],"销售数量",SUM('订单表'[数量]),"销售金额",SUM('订单表'[金额]),"利润金额",SUM('订单表'[利润])) 
```

![image-20230303170913288](https://gitee.com/zh_sng/cartographic-bed/raw/master/img/image-20230303170913288.png)
