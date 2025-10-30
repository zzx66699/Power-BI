## 📘 SUMX()

### 🧩 分类
`SUMX()` 属于 **Iterator functions（迭代函数）**，  
也可以归类为 **Aggregation functions（聚合函数）** 的扩展形式。  
它在 DAX 中用于对表中的每一行进行计算，然后将结果求和。

---

### 🧩 语法
```DAX
SUMX(<table>, <expression>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<table>` | 要迭代的表（可为实际表，也可为返回表的表达式）。 |
| `<expression>` | 针对表中每一行执行的表达式（通常是乘法、条件计算或其他度量）。 |

---

### 💡 功能说明
`SUMX()` 会：
1. 遍历 `<table>` 中的每一行；
2. 对每一行计算 `<expression>`；
3. 将所有结果相加并返回一个标量（单一数值）。

📘 换句话说：
> `SUMX()` = “按行计算，再汇总结果”。  
> 相比 `SUM()`，它能对每行执行更复杂的逻辑，而不仅仅是加总某一列。

---

### 📊 示例 1️⃣ — 基本示例
```DAX
Total Sales :=
SUMX(
    Sales,
    Sales[Quantity] * Sales[UnitPrice]
)
```
👉 逻辑：
- 对 `Sales` 表的每一行计算 `Quantity × UnitPrice`；
- 然后把所有行的结果相加；
- 得到整张表的总销售额。

| Quantity | UnitPrice | RowTotal | 累计 |
|-----------|------------|----------|------|
| 2 | 10 | 20 | 20 |
| 3 | 15 | 45 | 65 |
| 1 | 8 | 8 | **73** ✅ |

---

### 📊 示例 2️⃣ — 带筛选逻辑
```DAX
High Value Sales :=
SUMX(
    FILTER(Sales, Sales[UnitPrice] > 100),
    Sales[Quantity] * Sales[UnitPrice]
)
```

👉 逻辑：
1. 先过滤出单价高于 100 的行；
2. 对每行计算 `Quantity × UnitPrice`；
3. 汇总后返回总额。

---

### 📊 示例 3️⃣ — 按客户计算小计
```DAX
Customer Revenue :=
SUMX(
    VALUES(Sales[CustomerKey]),
    CALCULATE(SUM(Sales[SalesAmount]))
)
```
👉 含义：
- 先取出每个唯一客户；
- 计算该客户的销售额；
- 汇总所有客户的销售总额。

---

### ⚙️ 与 SUM() 的区别

| 对比项 | `SUM()` | `SUMX()` |
|---------|----------|-----------|
| 参数 | 只能是单列 | 可以是任意表达式 |
| 是否逐行迭代 | ❌ 否 | ✅ 是 |
| 可执行复杂逻辑 | ❌ 不可以 | ✅ 可以 |
| 性能 | 更快 | 稍慢（但更灵活） |
| 示例 | `SUM(Sales[Amount])` | `SUMX(Sales, Sales[Qty]*Sales[Price])` |

---

### 📚 常见的 X 系列迭代函数
| 函数 | 含义 |
|------|------|
| `SUMX()` | 对每行计算后求和 |
| `AVERAGEX()` | 对每行计算后取平均 |
| `MINX()` | 取最小值 |
| `MAXX()` | 取最大值 |
| `COUNTX()` | 统计非空结果数 |
| `RANKX()` | 对每行计算排名 |

这些函数都有相同结构：  
> `FunctionX(<table>, <expression>)`

---

### 🧠 性能优化建议
- 在 `<table>` 中尽量避免包含太多列，传入**精简表**或 `VALUES()` 结果；
- 若 `<expression>` 可提前在模型中计算（如添加计算列），则优先；
- 对大数据集使用 `SUMX` 时，可结合 `FILTER()` 或 `VAR` 限定计算范围。

---

### ✅ 一句话总结
> `SUMX()` 是 DAX 中的“按行汇总”函数，  
> 它在遍历表中每一行后，执行自定义计算并对结果求和。  
> 与 `SUM()` 相比，`SUMX()` 能处理更复杂的逐行逻辑，是建模中非常常用的核心函数。
