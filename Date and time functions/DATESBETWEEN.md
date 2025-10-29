## 📘 DATESBETWEEN()

### 🧩 语法
```DAX
DATESBETWEEN(<dates>, <start_date>, <end_date>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<dates>` | 日期列（通常为 `'Date'[Date]`）。 |
| `<start_date>` | 起始日期，定义范围的开始。 |
| `<end_date>` | 结束日期，定义范围的结束。 |

---

### 💡 功能说明
- `DATESBETWEEN()` 返回一个**日期表**，包含 `<dates>` 列中介于 `<start_date>` 与 `<end_date>` 之间的所有日期（包括起止边界）。  
- 日期必须存在于 `<dates>` 列中；若日期表中缺天，返回结果也会缺对应日期。

---

### 🧠 常见用途
| 用途 | 示例 |
|------|------|
| 计算自定义时间段的销售 | `CALCULATE([Sales], DATESBETWEEN(...))` |
| 定义滚动区间（如最近 30 天） | `DATESBETWEEN('Date'[Date], TODAY()-30, TODAY())` |
| 自定义财务报告周期 | `DATESBETWEEN('Date'[Date], [FiscalStart], [FiscalEnd])` |

---
### 📊 示例

假设 `'Date'[Date]` 包含以下日期：

| Date |
|------|
| 2025-01-01 |
| 2025-01-02 |
| 2025-01-04 |
| 2025-01-05 |

#### 示例 1
```DAX
DATESBETWEEN('Date'[Date], DATE(2025,1,2), DATE(2025,1,4))
```
返回的表为：
| Date       |
| ---------- |
| 2025-01-02 |
| 2025-01-04 |

> 只会返回 'Date'[Date] 中所有满足2025-01-02 ≤ Date ≤ 2025-01-04 的行。

---
#### 示例 2
```DAX
DATESBETWEEN('Date'[Date], DATE(2025,1,6), DATE(2025,1,10))
```
返回的表为：
| Date    |
| ------- |
| *(无数据)* |

---

#### 📊 示例 3：结合 CALCULATE 使用
```DAX
Sales Jan 2–4 :=
CALCULATE(
    [Total Sales],
    DATESBETWEEN('Date'[Date], DATE(2025,1,2), DATE(2025,1,4))
)
```
👉 返回 2025-01-02 到 2025-01-04 之间的销售总额。

---
