## 🧩 COUNT
### 📘 语法
```DAX
COUNT(<columnName>)
```
- `COUNT()` 返回指定列中**非空（non-blank）数值单元格的数量**。  
- 它只统计**数值类型**（Number、Currency、Date/Time 等）的列。  
- 如果列中包含文本或布尔值，`COUNT()` 会忽略这些行。  
- 若要统计所有非空值（包括文本、日期等），应使用 `COUNTA()`。  
- 常用于：
  - 统计记录数量（例如交易笔数、订单行数）  
  - 检查数值字段中有多少非空项  
  - 与 `CALCULATE()` 搭配，用于在特定筛选条件下计数  

---

### 📊 示例
表：Sales  
| SaleID | ProductID | Quantity | SalesAmount |
|--------|------------|-----------|--------------|
| 1 | A101 | 2 | 200 |
| 2 | A102 | *(blank)* | 300 |
| 3 | A101 | 5 | 500 |
| 4 | A103 | *(blank)* | *(blank)* |

---

#### 示例 1：统计非空数量
```DAX
Count Quantity = COUNT(Sales[Quantity])
```

结果：
| Metric | Value |
|---------|--------|
| Count Quantity | 2 |

> 因为只有前两行的 `Quantity` 是数值（2, 5），其余行为空或非数值。

---

#### 示例 2：比较 `COUNT()` 与 `COUNTA()`
```DAX
CountA Quantity = COUNTA(Sales[Quantity])
```

结果：
| Metric | COUNT | COUNTA |
|---------|--------|--------|
| Quantity | 2 | 2 |

（在此示例中结果相同，但若列包含文本或逻辑值时，`COUNTA()` 仍会计数。）

---

#### 示例 3：结合 CALCULATE 使用
```DAX
High Quantity Count =
CALCULATE(
    COUNT(Sales[Quantity]),
    Sales[Quantity] > 3
)
```

结果：
| Metric | Value |
|---------|--------|
| High Quantity Count | 1 |

---
