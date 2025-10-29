## 📘 ENDOFMONTH()

### 🧩 语法
```DAX
ENDOFMONTH(<dates>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<dates>` | 一个日期列或日期表达式（通常是 `'Date'[Date]`）。 |

---

### 💡 功能说明
`ENDOFMONTH()` 返回一个**单列日期表**，  
该表包含 `<dates>` 列中当前筛选上下文内**最后一个日期（即当月月末）**。

---

### 📊 示例 1
假设你的日期表 `'Date'` 包含以下日期：

| Date |
|------|
| 2025-01-01 |
| 2025-01-15 |
| 2025-01-31 |
| 2025-02-10 |

在当前上下文为 **2025 年 1 月** 时：
```DAX
ENDOFMONTH('Date'[Date])
```

返回的表是：
| Date |
|------|
| 2025-01-31 |

---

### 📊 示例 2
结合 `CALCULATE()` 使用：
```DAX
Sales End of Month :=
CALCULATE(
    [Total Sales],
    ENDOFMONTH('Date'[Date])
)
```
👉 该度量会计算每个月最后一天的销售额。

---


### 📚 同类函数（Date and time functions）

| 函数 | 功能 |
|------|------|
| `STARTOFMONTH()` | 返回当前月份的第一天 |
| `ENDOFMONTH()` | 返回当前月份的最后一天 |
| `STARTOFYEAR()` | 返回当前年份的第一天 |
| `ENDOFYEAR()` | 返回当前年份的最后一天 |
| `STARTOFQUARTER()` | 返回当前季度的第一天 |
| `ENDOFQUARTER()` | 返回当前季度的最后一天 |

