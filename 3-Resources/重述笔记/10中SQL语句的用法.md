---
Project: 
Status:
tags: Resources
Deadline:
CreateTime:
Connected:
---


### 1. SELECT all columns with LIMIT
`SELECT` 命令会选择所有的行、列，全部进行打印时，浪费时间、电脑内存，`Limit`命令可以查看数据中的前几行。
**数据表：Customers**

| CustomerID | CustomerName | City     | Country  |
|------------|--------------|----------|----------|
| 1          | John Doe     | New York | USA      |
| 2          | Jane Smith   | Madrid   | Spain    |
| 3          | Juan Carlos  | Barcelona| Spain    |
| 4          | Sara Lee     | Paris    | France   |

**SQL 语句：**

```sql
SELECT * FROM Customers LIMIT 3;
```

**输出：**

| CustomerID | CustomerName | City     | Country |
|------------|--------------|----------|---------|
| 1          | John Doe     | New York | USA     |
| 2          | Jane Smith   | Madrid   | Spain   |
| 3          | Juan Carlos  | Barcelona| Spain   |


### 2. WHERE clause
`WHERE`可以对特定列的值进行过滤。例如，我们过滤来自西班牙的国家，并且返回对应的城市。
**数据表：Customers**

| CustomerID | CustomerName | City     | Country  |
|------------|--------------|----------|----------|
| 1          | John Doe     | New York | USA      |
| 2          | Jane Smith   | Madrid   | Spain    |
| 3          | Juan Carlos  | Barcelona| Spain    |
| 4          | Sara Lee     | Paris    | France   |


**SQL 语句：**

```sql
SELECT City 
FROM Customers
WHERE Country = 'Spain';
```

**输出：**

| City       |
|------------|
| Madrid     |
| Barcelona  |


### 3. GROUP BY and HAVING clause

当我们想要对数据中相同观测值进行分组时，我们可以使用 `GROUP BY`, `HAVING` 可以用来过滤加总的数据，常用的包括 **sum、count**。

> “ `HAVING` 用来处理加总数据，而`WHERE`用来处理非加总数据。


**数据表：world**

| Continent  | Country     | Population |
|------------|-------------|------------|
| Asia       | China       | 1400000000 |
| Asia       | India       | 1380000000 |
| Europe     | Germany     | 83000000   |
| Europe     | France      | 67000000   |
| Africa     | Nigeria     | 200000000  |

**SQL 语句：**

```sql
SELECT continent, SUM(population)
FROM world
GROUP BY continent
HAVING SUM(population) > 500000000;
```

**输出：**

| Continent | SUM(Population) |
|-----------|-----------------|
| Asia      | 2780000000      |


### 4. ORDER BY clause

`Order By` 将数据按照选择的列进行升序或者排列：

**数据表：world**

| Continent  | Country     | Population |
|------------|-------------|------------|
| Asia       | China       | 1400000000 |
| Asia       | India       | 1380000000 |
| Europe     | Germany     | 83000000   |
| Europe     | France      | 67000000   |
| Africa     | Nigeria     | 200000000  |


**SQL 语句：**

```sql
SELECT name, population
FROM world
WHERE population > 200000000
ORDER BY population DESC;
```

**输出：**

| Name       | Population  |
|------------|-------------|
| China      | 1400000000  |
| India      | 1380000000  |
| USA        | 331000000   |


### 5. Date Function

时间处理函数依赖于SQL的内核，不同SQL的时间处理函数有所差异，`DATEPART` 可以提取时间中的年月日。

**数据表：eclipse**

| whn            |
|----------------|
| 2023-04-20     |
| 2024-10-02     |
| 2025-04-08     |

**SQL 语句：**

```sql
SELECT whn,
DATEPART(YEAR, whn) AS yr,
DATEPART(MONTH, whn) AS mnth
FROM eclipse;
```

**输出：**

| whn        | yr   | mnth |
|------------|------|------|
| 2023-04-20 | 2023 | 4    |
| 2024-10-02 | 2024 | 10   |
| 2025-04-08 | 2025 | 4    |


### 6. Joins
`Joins` 包括笛卡尔积、内积、外积、Self Join，接下来我们将介绍 Inner、left join 和 right join 。

![](https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcQ0Vx6Gg6xRnNakuiMQ5373ho0-HNzqE05Bn_ze9n95yIt6uTcJ)


**数据表：Orders**

| OrderID | CustomerID | EmployeeID |
|---------|------------|------------|
| 101     | 1          | 1          |
| 102     | 2          | 3          |
| 103     | 3          | 2          |

**数据表：Customers**

| CustomerID | CustomerName |
|------------|--------------|
| 1          | John Doe     |
| 2          | Jane Smith   |
| 3          | Juan Carlos  |

**SQL 语句（Inner Join）：**

```sql
SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID
LIMIT 5;
```

**输出：**

| OrderID | CustomerName |
|---------|--------------|
| 101     | John Doe     |
| 102     | Jane Smith   |
| 103     | Juan Carlos  |

**SQL 语句（Left Join）：**

```sql
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CustomerName
LIMIT 5;
```

**输出：**

| CustomerName | OrderID |
|--------------|---------|
| Jane Smith   | 102     |
| John Doe     | 101     |
| Juan Carlos  | 103     |

### 7. CASE WHEN clause

`Case When` 可以让我们根据现有的数据生成一些新的列，类似于Python、Exce中的if-else语法。

**数据表：world**

| Continent  | Country     | Population |
|------------|-------------|------------|
| Asia       | China       | 1400000000 |
| Asia       | India       | 1380000000 |
| Europe     | Germany     | 83000000   |
| Europe     | France      | 67000000   |
| Africa     | Nigeria     | 200000000  |

**SQL 语句：**

```sql
SELECT name, population,
      CASE WHEN population < 1000000 THEN 'small'
           WHEN population < 10000000 THEN 'medium'
           ELSE 'large'
      END as population_bucket
FROM world;
```

**输出：**

| Name        | Population | Population_Bucket |
|-------------|------------|-------------------|
| China       | 1400000000 | large             |
| India       | 1380000000 | large             |
| Germany     | 83000000   | large             |
| Nigeria     | 200000000  | large             |


### 8. Subqueries
子查询是SQL查询中的一种高级用法，它是在一个查询语句内部嵌套另一个查询语句。子查询可以用于WHERE、FROM、SELECT等子句中，提供更复杂的查询逻辑。

**数据表：world**

| Continent  | Country     | Population |
|------------|-------------|------------|
| Asia       | China       | 1400000000 |
| Asia       | India       | 1380000000 |
| Europe     | Germany     | 83000000   |
| Europe     | France      | 67000000   |
| Africa     | Nigeria     | 200000000  |

**SQL 语句：**

```sql
SELECT name 
FROM world
WHERE population > 
(SELECT population FROM world WHERE name = 'Russia');
```

**输出：**

| Name   |
|--------|
| China  |
| India  |


### 9. Window function

窗口函数是SQL中的高级查询功能，用于在结果集的子集（窗口）上执行聚合或分析操作，而不改变原始数据行数。它们通过OVER子句定义窗口或范围，可以实现如分组内排序（RANK, DENSE_RANK, ROW_NUMBER）、累计求和、平均值等计算，且能保持所有行。

窗口函数包括以下三种：

- 加总函数（Aggregate functions）： SUM, AVG, MAX, MIN等
- 排序函数（Ranking functions）： RANK, ROW_NUMBER等
- 分析函数（Analytic functions）：LEAD, LED等
**数据表：ge**

| yr   | party   | votes | constituency  |
|------|---------|-------|---------------|
| 2020 | Party A | 1000  | S14000021     |
| 2020 | Party B | 1500  | S14000021     |
| 2021 | Party A | 1200  | S14000021     |
| 2021 | Party B | 1600  | S14000021     |

**SQL 语句：**

```sql
SELECT yr, party, votes,
RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
FROM ge
WHERE constituency = 'S14000021'
ORDER BY party, yr;
```

**输出：**

| yr   | party   | votes | posn |
|------|---------|-------|------|
| 2020 | Party A | 1000  | 2    |
| 2020 | Party B | 1500  | 1    |
| 2021 | Party A | 1200  | 2    |
| 2021 | Party B | 1600  | 1    |


### 10. Union

`Union` 常常用来竖直方向组合多个数据集，输入的数据应该具有以下两个特征：

- 列的的名字和数量是形同的
- 每列的数据类型是相同的

**数据表：Suppliers**

| SupplierID | SupplierName | City      |
| ---------- | ------------ | --------- |
| 1          | Supplier1    | Madrid    |
| 2          | Supplier2    | Barcelona |
|            |              |           |

**数据表：Customers**

| CustomerID | CustomerName | City     | Country  |
|------------|--------------|----------|----------|
| 1          | John Doe     | New York | USA      |
| 2          | Jane Smith   | Madrid   | Spain    |
| 3          | Juan Carlos  | Barcelona| Spain    |
| 4          | Sara Lee     | Paris    | France   |

**SQL 语句：**

```sql
SELECT DISTINCT City
FROM 
(SELECT City FROM Customers
UNION
SELECT City FROM Suppliers)
LIMIT 5;
```

**输出：**

| City      |
|-----------|
| New York  |
| Madrid    |
| Barcelona |
| Paris     |
