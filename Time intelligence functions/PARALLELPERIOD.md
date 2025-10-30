## 📘 PARALLELPERIOD()

### 🧩 语法
```DAX
PARALLELPERIOD(<dates>, <number_of_intervals>, <interval>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<dates>` | 日期列（通常为 `'Date'[Date]`）。 |
| `<number_of_intervals>` | 要偏移的时间间隔数量，可为正（向未来）或负（向过去）。 |
| `<interval>` | 时间单位，可取以下关键字：`MONTH`、`QUARTER`、`YEAR`。 |

---

### 💡 功能说明
> `PARALLELPERIOD()` 返回一个**日期表**，  
> 该表表示相对于当前筛选上下文，**平移指定时间间隔后的日期范围**。  
> PARALLELPERIOD always returns complete periods at the specified granularity, whereas DATEADD may include partial periods. For example, if you have a selection of dates spanning from June 10 to June 21 within the same year and want to shift it forward by one month, PARALLELPERIOD will return all dates from the following month (July 1 to July 31). However, if DATEADD is used instead, the result will comprise only the dates from July 10 to July 21.
---

### 📊 示例 1️⃣ — 上个月的日期表
```DAX
PARALLELPERIOD('Date'[Date], -1, MONTH)
```

如果当前上下文是 2025 年 3 月的所有日期，  
则返回的日期范围为 2025 年 2 月的所有日期。

| 当前上下文 | 返回结果 |
|-------------|------------|
| 2025-03-01 → 2025-03-31 | 2025-02-01 → 2025-02-28 |

---

### 📊 示例 2️⃣ — 去年的日期表
```DAX
PARALLELPERIOD('Date'[Date], -1, YEAR)
```

若当前上下文是 2025 年 3 月，  
则返回 2024 年 3 月的日期范围。

---

### 📊 示例 3️⃣ — 与 CALCULATE 配合使用（去年同期销售）
```DAX
Sales Last Year :=
CALCULATE(
    [Total Sales],
    PARALLELPERIOD('Date'[Date], -1, YEAR)
)
```

🔹 含义：  
在每个日期上下文下，计算与当前时间**平行的上一年时间段**的销售额。

---

### ⚠️ 注意事项
- 返回值为**表 (table)**，用于 `CALCULATE()`、`CALCULATETABLE()` 等函数的筛选输入。  
- `<dates>` 必须来自一个**连续日期表（连续无缺口）**，否则可能报错或返回不完整。  
- `<interval>` 参数必须是固定的时间单位（MONTH / QUARTER / YEAR），不能使用 WEEK。  
---

### 📚 同类函数（Time intelligence functions）

| 函数 | 功能 |
|------|------|
| `SAMEPERIODLASTYEAR()` | 返回上一年相同日期范围 |
| `DATEADD()` | 将当前上下文向前/向后移动固定间隔 |
| `PARALLELPERIOD()` | 返回平移 N 个周期的“平行时间段” |
| `PREVIOUSMONTH()` | 返回上一个月 |
| `PREVIOUSYEAR()` | 返回上一年 |
