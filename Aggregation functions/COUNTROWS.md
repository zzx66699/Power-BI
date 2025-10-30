## 🧩 COUNTROWS
### 📘 语法
```DAX
COUNTROWS(<table>)
```
- `COUNTROWS()` 返回指定表中的**行数**（Row Count）。  
- 统计的对象是**表（table）**，而不是某一列，因此它会计算该表当前筛选上下文下的行总数。
- COUNTROWS() 计算的是表的行数（Row Count），它不会检查每列是否有值，所以会计算包含空值的行(即使所有列都是空的）。  
- 常用于：
  - 统计记录条数（如订单行数、销售笔数、交易数）  
  - 结合 `FILTER()`、`RELATEDTABLE()`、`DISTINCT()` 等生成条件表后计数  
  - 构建 KPI、唯一记录数、活跃用户数等指标

---

### 📊 示例
表：Sales  
| SaleID | Product | Customer | SalesAmount |
|--------|----------|-----------|--------------|
| 1 | A101 | John | 200 |
| 2 | A102 | Mary | 300 |
| 3 | A101 | John | 150 |
| 4 | A103 | Alex | 400 |
| 5 | A102 | *(blank)* | 100 |

---

#### 示例 1：统计 Sales 表的总行数
```DAX
Total Rows = COUNTROWS(Sales)
```

结果：
| Metric | Value |
|---------|--------|
| Total Rows | 5 |

---

#### 示例 2：结合 FILTER() 统计特定条件的行
```DAX
High Sales Count =
COUNTROWS(
    FILTER(
        Sales,
        Sales[SalesAmount] > 200
    )
)
```

结果：
| Metric | Value |
|---------|--------|
| High Sales Count | 3 |

> 说明：只有行 2、4、5 的 SalesAmount > 200。

---

#### 示例 3：结合 DISTINCT() 统计唯一客户数
```DAX
Unique Customers =
COUNTROWS(DISTINCT(Sales[Customer]))
```

结果：
| Metric | Value |
|---------|--------|
| Unique Customers | 4 |

---

### 💬 说明
- `COUNTROWS()` 是对表行数的统计函数，不关心列类型。  
- 若表为空，返回 0。  
- 可直接用于物理表，也可用于虚拟表（如 `FILTER()`、`ALL()`、`RELATEDTABLE()`、`SUMMARIZE()` 的结果）。  
- 常与 `CALCULATE()` 搭配，用于在特定上下文中计数。  
- 与 `COUNT()` 不同，它不要求传入列，也不会忽略空值。  

---
