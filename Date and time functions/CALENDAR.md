## 🧩 CALENDAR()

### 📘 语法
```DAX
CALENDAR(<start_date>, <end_date>)
```

### 📖 功能
返回一个包含从 `<start_date>` 到 `<end_date>` 的所有日期的单列表（列名为 `Date`）。

### 📂 分类
Date and Time functions

---

### 📊 示例
```DAX
DateTable =
CALENDAR(DATE(2020,1,1), DATE(2020,12,31))
```
结果表：

| Date |
|------|
| 2020-01-01 |
| 2020-01-02 |
| 2020-01-03 |
| ... |
| 2020-12-31 |

---

### 💡 常见用途
- 创建时间智能函数（如 `TOTALYTD`、`SAMEPERIODLASTYEAR`）所需的日期表。
- 用作模型中的 **Date dimension（日期维度表）**。
- 搭配 `ADDCOLUMNS()` 扩展更多日期字段（如 Year、Month、Quarter）。

示例：
```DAX
DateTable =
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2025,12,31)),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Month Number", MONTH([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q")
)
```

---

### ⚠️ 注意
- `CALENDAR()` 只接受固定的开始和结束日期；
- 如果需要根据数据自动计算日期范围，请使用 `CALENDARAUTO()`；
- 返回的表只有一列（`Date`），需要用 `ADDCOLUMNS()` 添加其他列。

---

### 🧠 相关函数对比

| 函数 | 功能 |
|------|------|
| `CALENDAR()` | 根据指定起止日期生成日期表 |
| `CALENDARAUTO()` | 自动根据模型中日期列的最小和最大日期生成日期表 |
