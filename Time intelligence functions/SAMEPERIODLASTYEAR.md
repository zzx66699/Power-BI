## 📘 SAMEPERIODLASTYEAR()

### 🧩 语法
```DAX
SAMEPERIODLASTYEAR(<dates>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<dates>` | 日期列（通常为 `'Date'[Date]`）。必须来自连续日期表。 |

---

### 💡 功能说明
`SAMEPERIODLASTYEAR()` 返回一张日期表，  
该表表示当前筛选上下文中日期列的相同时间段，但偏移到**上一年**。  
常用于 **同比（Year-over-Year, YoY）分析**。   
函数返回的日期集等同于`DATEADD('Date'[Date], -1, YEAR)`

---

### 📊 示例 

#### 示例 1️⃣ — 单日上下文
当前上下文：`2025-03-10`  
```DAX
SAMEPERIODLASTYEAR('Date'[Date])
```
👉 返回：
```
2024-03-10
```

---

####  示例 2️⃣ — 月度上下文
当前上下文：`2025-03-01 → 2025-03-31`  
```DAX
SAMEPERIODLASTYEAR('Date'[Date])
```
👉 返回：
```
2024-03-01 → 2024-03-31
```

---

#### 示例 3️⃣ — 年度同比销售额
```DAX
Sales Last Year :=
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Date'[Date])
)
```

👉 含义：  
对每个当前日期上下文（例如某月、某季度或某天），  
计算“去年同期”的销售额。  
这是 DAX 中计算 **YoY（同比）** 的最经典写法。

---


### 📚 同类函数（Time intelligence functions）

| 函数 | 功能 |
|------|------|
| `SAMEPERIODLASTYEAR()` | 返回上一年的相同时间段 |
| `PARALLELPERIOD()` | 返回平移 N 个周期的相同长度时间段 |
| `DATEADD()` | 逐日期偏移 |
| `PREVIOUSYEAR()` | 返回上一整年的日期表 |
| `PREVIOUSMONTH()` | 返回上一个月的日期表 |
| `PREVIOUSQUARTER()` | 返回上一个季度的日期表 |
