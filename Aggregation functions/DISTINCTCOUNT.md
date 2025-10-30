## 🧩 DISTINCTCOUNT
### 📘 语法
```DAX
DISTINCTCOUNT(<column>)
```
- `DISTINCTCOUNT()` 返回指定列中**唯一（不重复）**值的数量。  
- 若列中包含空值（blank），该空值会被 **计入 1 次**（即每个空值只计一次）。
- 如果不想记录空值，可以用DISTINCTCOUNTNOBLANK(<column>)
- 与 `COUNT()` 不同，它不要求列为数值类型。 
- 常用于：
  - 统计唯一客户数、唯一订单数、唯一产品数等  
  - 结合 `CALCULATE()` 用于不同筛选条件下的唯一计数  
  - 创建 KPI 或百分比指标（如“活跃客户占比”）

---

### 📊 示例
表：Sales  
| Customer | Product | SalesAmount |
|-----------|----------|--------------|
| John | A101 | 200 |
| Mary | A102 | 300 |
| John | A103 | 150 |
| Alex | A101 | 250 |
| *(blank)* | A102 | 100 |

---

#### 示例 1：统计唯一客户数量
```DAX
Unique Customers = DISTINCTCOUNT(Sales[Customer])
```

结果：
| Metric | Value |
|---------|--------|
| Unique Customers | 4 |

> 说明：John、Mary、Alex、(blank) 共四个唯一值。

---

#### 示例 2：比较 DISTINCTCOUNT 与 COUNTROWS(DISTINCT())
```DAX
Unique Customers (Manual) =
COUNTROWS(DISTINCT(Sales[Customer]))
```

结果与 `DISTINCTCOUNT()` 完全相同，只是写法更长。  

---

#### 示例 3：结合 CALCULATE 使用
```DAX
Unique Customers (Laptop) =
CALCULATE(
    DISTINCTCOUNT(Sales[Customer]),
    Sales[Product] = "A101"
)
```

结果：
| Metric | Value |
|---------|--------|
| Unique Customers (Laptop) | 2 |

---

