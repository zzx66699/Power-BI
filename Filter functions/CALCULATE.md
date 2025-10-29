## 🧩 CALCULATE
### 📘 语法
```DAX
CALCULATE(<expression>, <filter1>[, <filter2>], ...)
```
- `CALCULATE()` 是 DAX 中最核心的函数，用于**在修改筛选上下文（Filter Context）后重新计算表达式**。
- `CALCULATE()` 是唯一可以**修改筛选上下文**的函数。   
- 若无筛选条件，仅使用 `CALCULATE(<expression>)`，结果等同于原表达式。
- `CALCULATE`的筛选条件返回的要么是“列=值” 的简单形式，要么必须是一个**虚拟表**。   
- 常用于：
  - 按特定条件计算聚合（如“仅计算 Laptop 类别销售额”）；  
  - 创建动态百分比、同比环比等度量；  
- 常见组合：
  - `CALCULATE(SUM(...), ALL(...))` → 忽略筛选；
  - `CALCULATE(SUM(...), FILTER(...))` → 条件筛选；
  - `CALCULATE([Measure], VALUES(...))` → 按当前维度重新计算。

---

### 📊 示例
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
> DAX引擎自动识别这一句的语义：“在当前模型中，对 Sales[Category] 这一列添加一个筛选：只保留值等于 "Laptop" 的行。”  
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
> CALCULATE 把这个虚拟表的结果“变成新的筛选上下文”，意思是从现在起，只考虑 Sales 表中出现在这张虚拟表里的行。
> 现在的 Sales，已经不再指向完整表，而是“经过 FILTER() 筛选之后的那部分行”。
> 💡CALCULATE 的简单过滤条件只能是 “列 = 某个常量值”，只要你写了比较运算符（>、<、>=、<=、<>），就必须用 FILTER()。
---

#### 示例 2：计算所有类别的总销售额（忽略筛选）
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

> `ALL()` 移除了 Category 的筛选，因此始终返回全表总额。 
