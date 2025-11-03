## 📘 CALENDARAUTO()

### 🧩 语法
```DAX
CALENDARAUTO([fiscal_year_end_month])
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `[fiscal_year_end_month]` *(可选)* | 财政年度的结束月份（1–12）。如果省略，默认值为 12（即自然年）。 |

---

### 💡 功能说明
`CALENDARAUTO()` 会在你的 Power BI 模型中：
1. 搜索所有包含日期或日期时间类型的列；  
2. 找出这些列中最早和最晚的日期；  
3. 自动创建一个从该最早日期到最晚日期之间、**无缺口的连续日期表**。并自动补齐全年。

---

### 📊 示例 1️⃣ — 默认自然年
```DAX
Date =
CALENDARAUTO()
```
如果你的数据模型中最早的销售日期是 `2019-03-15`，  
最晚日期是 `2025-09-30`，  
则 `CALENDARAUTO()` 会自动返回日期范围：
```
2019-01-01 → 2025-12-31
```
（按自然年对齐，自动补全年初和年末。）

---

### 📊 示例 2️⃣ — 财政年度从 7 月开始
```DAX
Date =
CALENDARAUTO(6)
```
如果财政年度在 **6 月结束**（即每年 7 月开始新财年），  
而你模型中最晚日期是 `2025-09-30`，  
则生成的日期表会覆盖完整的财年范围：
```
2018-07-01 → 2026-06-30
```

---

### 📊 示例 3️⃣ — 生成日期表并添加辅助列
```DAX
Date =
ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Month", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT(QUARTER([Date]), "0"),
    "Weekday", WEEKDAY([Date])
)
```

👉 这样得到的 `Date` 表包含完整的连续日期 + 年/月/季度等常用字段，  
可以直接用作 Power BI 模型的日期维度表。

---

### ⚙️ 注意事项
- `CALENDARAUTO()` 生成的日期表是 **连续的（contiguous）**，  
  因此可以安全地用于时间智能函数（如 `DATESYTD()`、`SAMEPERIODLASTYEAR()` 等）。  
- 返回的表有 **单列 `[Date]`**，每行代表一个日期。  
