## 📘 TOTALYTD()

### 🧩 语法
```DAX
TOTALYTD(<expression>, <dates>, [<filter>], [<year_end_date>])
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<expression>` | 要计算的度量或聚合表达式（通常是一个度量，比如 [Total Sales]）。 |
| `<dates>` | 日期列（通常是日期维度表中的日期列，如 'Date'[Date]）。 |
| `[<filter>]` *(可选)* | 附加筛选条件，例如 `'Product'[Category] = "A"`。 |
| `[<year_end_date>]` *(可选)* | 财政年度的结束日期，默认为 12 月 31 日。可设置如 `"06/30"` 表示财年在 6 月结束。 |

---

### 💡 功能说明
- `TOTALYTD()` 会自动计算**从当年年初到当前上下文日期**之间的累计值（Year-To-Date）。
- 在当前筛选上下文中，将日期过滤范围扩展为「本年度开始 → 当前日期」，再计算指定表达式的总和。  
- 所有时间智能函数（如 TOTALYTD, DATESYTD, SAMEPERIODLASTYEAR, DATESMTD 等），内部其实都会自动调用 CALCULATE()。  

---

### 📊 示例

假设有以下数据：

| Date | Sales |
|------|-------|
| 2025-01-01 | 100 |
| 2025-02-01 | 200 |
| 2025-03-01 | 300 |

定义度量：
```DAX
Total Sales := SUM(Sales[Sales])
YTD Sales := TOTALYTD([Total Sales], 'Date'[Date])
```

结果：

| Date | Total Sales | YTD Sales |
|------|--------------|-----------|
| 2025-01-01 | 100 | 100 |
| 2025-02-01 | 200 | 300 |
| 2025-03-01 | 300 | 600 |

---

### ⚙️ 等价写法
```DAX
YTD Sales :=
CALCULATE(
    [Total Sales],
    DATESYTD('Date'[Date])
)
```

> `TOTALYTD()` 实际上是 `CALCULATE + DATESYTD()` 的简写形式。

