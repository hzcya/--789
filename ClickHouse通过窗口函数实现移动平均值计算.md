在做数据分析时，经常会用到移动平均值这一概念，如：(x1+x2+x3)/3, (x2+x3+x4)/3, (x3+x4+x5)/3, ... 就是移动平均值，其中x可以表示日或者月，即3日移动平均值或3月移动平均值，经常会出现在如股票分析的应用中。那么，如何在Clickhouse中实现移动平均值的计算？

方法一：Clickhouse自带了丰富的库函数，其中groupArrayMovingAvg()函数就可以实现移动平均值的计算。

1、创建测试表wt_test_1：

CREATE TABLE wf_test_1 (
    id          UInt64,
    price       Decimal(16,2)
)
ENGINE = MergeTree
ORDER BY id;
2、表中数据如下所示：



3、使用groupArrayMovingAvg()函数计算price的移动平均值：

SELECT groupArrayMovingAvg(3)(price)
FROM wf_test_1;
其中参数3表示窗口大小为3，price为需要聚合的列。



该SQL执行结果为包括当前行在内的前3行的平均值，如id = 1的行，前两行不存在，计算结果为1.00 / 3 =0.33；如id = 3的行， 前两行包括当前行的平均值为 (1.00 + 2.00 + 3.00) /3 = 2.00。

如果不指定窗口大小为3，默认的窗口大小为总行数，该表的总行数为6，如id = 1的行，计算结果为1.00 / 6 = 0.16。

SELECT groupArrayMovingAvg(price)
FROM wf_test_1;


方案二：虽然方案一提供了现成的函数，使用起来非常方便，但是要求数据是有序的，而且窗口范围只能是包括当前行在内的之前的行，在实际应用中比较少。例如有这样一张表，id为物品编号，price为物品价格，data_date为录入价格的日期。现在我们需要计算每一个物品当天价格和前一天以及后一天价格的平均值，且求平均值时忽略掉价格为空的那一天，如前一天的价格为空，则只计算当天和后一天价格的平均值。这个需求方案一就无法实现了，我们可以通过单表自关联来实现这个计算需求。

1、创建测试表：

CREATE TABLE wf_test_2 (
    id          UInt64,
    price       Nullable(Decimal(16,2)),
    data_date   DateTime
)
ENGINE = MergeTree
ORDER BY id;
2、表中数据如下所示：



3、对该表进行自关联实现上述计算：

SELECT
    t.id,
    t.data_date,
	t.price,
    (assumeNotNull(t.price) + assumeNotNull(t1.price) + assumeNotNull(t2.price)) / (isNotNull(t.price) + isNotNull(t1.price) + isNotNull(t2.price)) AS three_days_avg
FROM wf_test_2 AS t
LEFT JOIN wf_test_2 AS t1 ON (t.id = t1.id) AND (toYYYYMMDD(t1.data_date) = toYYYYMMDD(t.data_date - toIntervalDay(1)))
LEFT JOIN wf_test_2 AS t2 ON (t.id = t2.id) AND (toYYYYMMDD(t2.data_date) = toYYYYMMDD(t.data_date + toIntervalDay(1)))
ORDER BY
    t.id ASC,
    t.data_date ASC;


方案三：方案二用到了单表的自关联，其实无论是单表自关联还是多表关联，其原理都是一样的，而Clickhouse的join能力又特别有限。在大数据量的情况下，方案二会耗时很久，也可能因为内存不足而无法完成任务，查询不出结果。所以，在实际的生产环境中，往往面临很大的数据量，我们可以通过窗口函数来实现同样的计算，从而避免在Clickhouse中使用join。SQL如下，其中OVER前的表达式必须是聚合函数，这里我们用的是avg()函数，求平均值；OVER后面的表达式为要进行聚合的分组及窗口，这里PARTITION BY id表示以id为分组，ORDER BY data_date ASC表示在每个分组内按data_date从小到大排序，ROWS 后面为窗口范围，BETWEEN 1 PRECEDING AND 1 FOLLOWING表示从前1行到后1行。

SELECT
    id,
    data_date,
    avg(price) OVER (PARTITION BY id ORDER BY data_date ASC ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS three_days_avg
FROM wf_test_2;
该SQL执行结果与方案二的执行结果相同，但是我们避免了join的使用，在大数据量的情况下，能够大大提升查询性能：

