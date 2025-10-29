## 🧩 VALUES
### 📘 语法
```DAX
VALUES(<columnName or table>)
```
`VALUES()` 返回指定列或表中**唯一值的列表**（即去重后的表）。  
它的作用与 `DISTINCT()` 类似，但在**筛选上下文（Filter Context）**中行为略有不同。  

📌 常用于：在筛选器或 `CALCULATE` 中，重新定义筛选范围  

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
VALUES(Sales[ProductID])
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

### 💡 解释
- `VALUES()` 会根据**当前筛选上下文**返回不同结果。  
  例如：  
  - 在整个数据范围内 → 返回所有唯一值。  
  - 在特定切片（如 Category="Laptop"）下 → 只返回该筛选条件下的唯一值。  
- 如果某列在当前上下文中没有任何值，`VALUES()` 会返回空表。  
- 当当前上下文只包含一个唯一值时，`VALUES()` 返回的表中也只有这一行。  
- `VALUES()` 在上下文计算中比 `DISTINCT()` 更灵活，适合与 `CALCULATE`、`FILTER` 等函数搭配使用。

---

### 🧠 小结
| 函数 | 返回类型 | 筛选上下文敏感 | 常见用途 |
|------|------------|------------------|-----------|
| `DISTINCT()` | 表 | ❌ 否 | 返回唯一值列表，忽略上下文 |
| `VALUES()` | 表 | ✅ 是 | 返回当前上下文下的唯一值 |
| `ALL()` | 表 | ✅ 是（但移除筛选） | 移除所有筛选，返回整列唯一值 |
