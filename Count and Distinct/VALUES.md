## 🧩 VALUES
### 📘 语法
```DAX
VALUES(<columnName or table>)
```
- `VALUES()` 返回指定列或表中**唯一值的列表**（即去重后的表）。  
- 它的作用与 `DISTINCT()` 类似，但会根据**当前筛选上下文**返回不同结果。  
  - 在整个数据范围内 → 返回所有唯一值。  
  - 在特定切片（如 Category="Laptop"）下 → 只返回该筛选条件下的唯一值。  
  - 如果某列在当前上下文中没有任何值，`VALUES()` 会返回空表。   
  - `VALUES()` 在上下文计算中比 `DISTINCT()` 更灵活，适合与 `CALCULATE`、`FILTER` 等函数搭配使用。
- 常用于在筛选器或 `CALCULATE` 中，重新定义筛选范围。
- 会保留blank（空值）作为一行

---

### 📊 示例
表：Sales  
| SaleID | ProductID | Customer | Quantity |
|--------|------------|-----------|-----------|
| 1 | A101 | John | 2 |
| 2 | A102 | Mary | 1 |
| 3 | A101 | John | 3 |
| 4 | A103 | Alex | 1 |
| 5 | A102 | Mary | 2 |

---

#### 示例 1：返回唯一的 ProductID
```DAX
ProductID = VALUES(Sales[ProductID])
```

结果（表）：
| ProductID |
|------------|
| A101 |
| A102 |
| A103 |

---

#### 示例 2：统计当前上下文中的唯一客户数
```DAX
Customer Count = COUNTROWS(VALUES(Sales[Customer]))
```

结果：
| Metric | Value |
|---------|--------|
| Customer Count | 3 |

---
#### 示例 3：返回含有blank的唯一值
表：Sales  
| Customer  | SalesAmount |
| --------- | ----------- |
| John      | 200         |
| Mary      | 300         |
| *(blank)* | 150         |
| John      | 250         |

```
Costomer = VALUES(Sales[Customer])
```

结果：
| Customer  |
| --------- |
| John      |
| Mary      |
| *(blank)* |



