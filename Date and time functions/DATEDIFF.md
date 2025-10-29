## 📘 DATEDIFF()

### 🧩 语法
```DAX
DATEDIFF(<start_date>, <end_date>, <interval>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<start_date>` | 起始日期，可以是日期列、日期表达式或日期函数。 |
| `<end_date>` | 结束日期，可以是日期列、日期表达式或日期函数。 |
| `<interval>` | 间隔单位，定义返回结果的时间粒度。可取以下关键字之一： |

| 关键字 | 说明 |
|--------|------|
| `SECOND` | 秒 |
| `MINUTE` | 分钟 |
| `HOUR` | 小时 |
| `DAY` | 天 |
| `WEEK` | 周（7 天为一周） |
| `MONTH` | 月 |
| `QUARTER` | 季度 |
| `YEAR` | 年 |

---

### 💡 功能说明
`DATEDIFF()` 用于计算两个日期之间的差值，  
返回结果为一个整数，表示以指定单位计的时间间隔。

---

### 📊 示例

| 表达式 | 结果 | 说明 |
|---------|--------|------|
| `DATEDIFF(DATE(2025,1,1), DATE(2025,1,31), DAY)` | `30` | 两日期相差 30 天 |
| `DATEDIFF(DATE(2024,1,1), DATE(2025,1,1), YEAR)` | `1` | 相差 1 年 |
| `DATEDIFF(DATE(2025,1,1), DATE(2025,4,1), MONTH)` | `3` | 相差 3 个月 |
| `DATEDIFF('Customer'[SignupDate], TODAY(), DAY)` | 计算注册至今的天数 |
