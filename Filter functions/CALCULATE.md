## 🧩 CALCULATE
### 📘 语法
```DAX
CALCULATE(<expression>, <filter1>[, <filter2>], ...)
```
- `CALCULATE()` 是 DAX 中最核心的函数，用于在修改筛选上下文后返回一个**标量（数值）**。 
- 若无筛选条件，仅使用 `CALCULATE(<expression>)`，结果等同于原表达式。
- `CALCULATE`过滤参数有两类：
  - 列筛选（column filter）：像切片器一样直接给某一列加筛选（例如 `'Product'[List Price] > 1000、'Date'[Year] = 2024`）。这类筛选不需要逐行遍历；引擎能把它编译成“给整列加一个范围/集合过滤”的索引条件。常见用于**维度表**的列，过滤会沿关系传播到事实表。或者是在事实表中简单的列=值的关系。  
  - 表筛选（table filter）：需要逐行判断的谓词，必须写成 FILTER(Table, condition) 返回一个“被保留下来的行的表”。比如`Sales[SalesAmount] > 200`，想筛掉事实表里的行，这是行级过滤，要用 `FILTER(Sales, Sales[SalesAmount] > 200`)。
  - 所以，<mark>维度列 → 可以直接当作 CALCULATE 的布尔过滤参数（列筛选）。事实表行条件 → 用 FILTER(事实表, 条件)（表筛选）。<mark>
- `CALCULATE`还可以将每个行的值变成筛选条件，从而将行上下文，转变成筛选上下文。这个功能叫做Perform context transition 执行上下文转换。  
- 常见组合：
  - `CALCULATE(SUM(...), ALL(...))` → 忽略筛选；
  - `CALCULATE(SUM(...), FILTER(...))` → 条件筛选；
  - `CALCULATE([Measure], VALUES(...))` → 按当前维度重新计算。

---

### 📊 示例 - 列筛选 vs. 表筛选
表：Sales  
| Product | Category | SalesAmount |
|----------|-----------|--------------|
| A | Laptop | 200 |
| B | Laptop | 300 |
| C | Mouse | 100 |
| D | Mouse | 400 |

---

#### 示例 1：计算 Laptop 类别的总销售额
```DAX
Laptop Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    Sales[Category] = "Laptop"
)
```

结果：
| Metric | Value |
|---------|--------|
| Laptop Sales | 500 |

> 说明：Category = "Laptop" 是简单的列=值的形式，这是DAX内置支持的“筛选定义语法”，所以CALCULATE可以直接识别。  
> 这种语法是结构化筛选（Structured Filter），DAX 会直接翻译成对“Category 列”的一个过滤上下文（Filter Context）。  
> **它不是在一行一行判断 True/False**，而是修改当前上下文：Category 列的允许值集合 = { "Laptop" }。 

---
#### 示例 2：结合 FILTER() 使用
```DAX
High Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    FILTER(Sales, Sales[SalesAmount] > 200)
)
```

结果：
| Metric | Value |
|---------|--------|
| High Sales | 700 |

> `FILTER()` 创建一个满足条件的虚拟表，这张虚拟表的结构与原表相同、表名也继承原表名，所以在逻辑上它还是 “Sales 表”。  
> CALCULATE 把这个虚拟表的结果“变成新的筛选上下文”。这张表被 CALCULATE 用来告诉 DAX 哪些行可见，然后 SUM 仍然在原始 Sales 表求和。  
> 💡CALCULATE 的简单过滤条件只能是 “列 = 某个常量值”，只要你写了比较运算符（>、<、>=、<=、<>），就必须用 FILTER()。  
---

#### 3：在维度表里筛选
```DAX
Revenue Expensive Products =
CALCULATE(
  SUM(Sales[SalesAmount]),
  Product[ListPrice] > 1000
)
```
> 这个之所以能直接用，是因为它对 Product（维度表） 做列筛选，随后通过关系过滤到 Sales。

----

### 📊 示例 - 忽略某个筛选条件 vs. 忽略所有筛选条件
表：Sales  
| Product | Category | SalesAmount |
|----------|-----------|--------------|
| A | Laptop | 200 |
| B | Laptop | 300 |
| C | Mouse | 100 |
| D | Mouse | 400 |

---
#### 示例 1：计算所有类别的总销售额
```DAX
All Category Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    ALL(Sales[Category])
)
```

结果：
| Metric | Value |
|---------|--------|
| All Category Sales | 1000 |

> `ALL()` 告诉 DAX 要把 Category 这列上的筛选条件清除掉，对模型中的原始 Sales 表 进行求和。  
> 但如果当前上下文有其他过滤条件，比如 Year = 2024 ，那么 Year 仍然是 2024。
---

#### 示例 4：计算所有销售额
```DAX
All Category Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    ALL(Sales)
)
```
> 忽略所有来自 Sales 表的过滤。

---

### 📊 示例 - 触发行上下文 → 转换为筛选上下文
表 CustomerRevenue
| Customer | Sale Amount |
| -------- | ----------- |
| A        | 1500        |
| A        | 1200        |
| B        | 800         |

---

#### ✅ 情况一：加了 CALCULATE()
```DAX
Customer Segment = 
VAR CustoemrRevenue = CALCULATE(SUM(CustomerRevenue[Sale Amount]))

RETURN
  IF(
    CustomerRevenue > 2500),
    "High",
    "Low"
)
```
> Step 1. 逐行计算（行上下文）  
当计算到第一行时，行上下文是：Customer = "A", Sale Amount = 1500  

> Step 2. 进入 CALCULATE()会做两件事：   
1️⃣ 触发行上下文 → 转换为筛选上下文  
DAX 会自动生成一个过滤条件：Customer = "A"  
所以现在 SUM() 能在这个上下文里看到所有 Customer = A 的行。  
2️⃣ 重新计算表达式  
计算 SUM(CustomerRevenue[Sale Amount])  
结果 = 1500 + 1200 = 2700 比较：2700 > 2500 → TRUE  

> Step 3. 返回 "High"  

结果：
| Customer | Sale Amount | 计算逻辑                                    | Class    |
| -------- | ----------- | --------------------------------------- | -------- |
| A        | 1500        | `SUM(A)` = 1500 + 1200 = 2700 → >2500 ✅ | **High** |
| A        | 1200        | `SUM(A)` = 1500 + 1200 = 2700 → >2500 ✅ | **High** |
| B        | 800         | `SUM(B)` = 800 → >2500 ❌                | **Low**  |

#### ❌ 情况二：没加 CALCULATE()
```DAX
Class :=
IF(
    SUM(CustomerRevenue[Sale Amount]) > 2500,
    "High",
    "Low"
)
```
| Customer | Sale Amount | 计算逻辑                           | Class   |
| -------- | ----------- | ------------------------------ | ------- |
| A        | 1500        | `SUM()` 只看当前行 = 1500 → >2500 ❌ | **Low** |
| A        | 1200        | `SUM()` 只看当前行 = 1200 → >2500 ❌ | **Low** |
| B        | 800         | `SUM()` 只看当前行 = 800 → >2500 ❌  | **Low** |






