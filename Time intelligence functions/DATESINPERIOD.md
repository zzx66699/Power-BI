## 📘 DATESINPERIOD()

### 🧩 语法
```DAX
DATESINPERIOD(<dates>, <start_date>, <number_of_intervals>, <interval>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<dates>` | 日期列（通常为 `'Date'[Date]`）。 |
| `<start_date>` | 起始日期，定义时间区间的锚点。 |
| `<number_of_intervals>` | 要偏移的时间数量（可以为正或负）。 |
| `<interval>` | 时间单位，可取以下关键字之一：`DAY`、`MONTH`、`QUARTER`、`YEAR`。 |

---

### 💡 功能说明
`DATESINPERIOD()` 返回一个**日期表**，  
包含从 `<start_date>` 开始、沿时间轴延伸 `<number_of_intervals>` 个 `<interval>` 单位的日期。

- 若 `<number_of_intervals>` 为正 → 向未来延伸  
- 若 `<number_of_intervals>` 为负 → 向过去延伸  

换句话说：
> 它根据起始日期和时间间隔，动态“向前或向后”生成一个日期范围。

---

### 📊 示例

假设 `'Date'[Date]` 包含以下日期：

| Date |
|------|
| 2025-01-01 |
| 2025-01-02 |
| 2025-01-03 |
| 2025-01-04 |
| 2025-01-05 |
| 2025-01-06 |
| 2025-01-07 |

---

#### 示例 1️⃣ — 最近 3 天
```DAX
DATESINPERIOD('Date'[Date], DATE(2025,1,05), -3, DAY)
```

👉 起始日期为 2025-01-05，  
👉 向过去 3 天（不含 5 日当天），返回范围：

| Date |
|------|
| 2025-01-02 |
| 2025-01-03 |
| 2025-01-04 |
| 2025-01-05 |

> 注意：起始日当天（1 月 5 日）也包含在内。

---

#### 示例 2️⃣ — 向后 1 个月
```DAX
DATESINPERIOD('Date'[Date], DATE(2025,1,01), 1, MONTH)
```
👉 从 2025-01-01 开始，往后 1 个月，  
返回 2025 年 1 月至 1 月末的所有日期。

---

### 📊 示例 3️⃣ — 与 CALCULATE 搭配（滚动区间）
```DAX
Sales Last 30 Days :=
CALCULATE(
    [Total Sales],
    DATESINPERIOD('Date'[Date], MAX('Date'[Date]), -30, DAY)
)
```

👉 在每个日期上下文下，返回过去 30 天内的销售额。  
这是一种常见的“滚动 30 天累计”写法。

---

### ⚙️ 注意事项
- 返回值是**日期表 (table)**，可用于 `CALCULATE()`、`CALCULATETABLE()` 等函数中。  
- 包含起始日期，
