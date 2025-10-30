## 📘 FILTER()

### 🧩 语法
```DAX
FILTER(<table>, <filter_expression>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<table>` | 要筛选的表（可以是模型中的物理表，也可以是返回表的表达式）。 |
| `<filter_expression>` | 对表中每一行进行逻辑测试的表达式。只保留计算结果为 `TRUE` 的行。 |

---

### 💡 功能说明
`FILTER()` 会：
1. 遍历 `<table>` 中的每一行；
2. 评估 `<filter_expression>`（条件表达式）；
3. 返回所有计算结果为 `TRUE` 的行组成的新表。

> ⚙️ 重要：`FILTER()` **不会修改原表**，  
> 而是返回一个新的“虚拟表 (virtual table)”。

---

### 📊 示例 1️⃣ - 与 ALL() 搭配，忽略现有筛选
```DAX
Total High Sales :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    FILTER(
        ALL(Sales),                      // 忽略外部筛选
        Sales[SalesAmount] > 1000
    )
)
```

👉 含义：  
无论外部筛选（如地区、时间）如何，  
都计算整张 Sales 表中金额 > 1000 的总和。
