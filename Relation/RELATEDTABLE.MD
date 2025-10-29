## 🧩 RELATEDTABLE
### 📘 语法
```DAX
RELATEDTABLE(<tableName>)
```
如果两个表有 relationship，`RELATEDTABLE` 会根据（多）表里的 Relationship Key 关系键进行查找，  
并在（⼀）表中返回与当前行相关的（多）表记录组成的**表**。  

它的作用方向与 `RELATED()` 相反：  
- `RELATED()`：从**一侧表**取出单个值到多侧表。  
- `RELATEDTABLE()`：从**多侧表**取出所有相关行到一侧表。  

---

### 📊 示例
表1：Sales（多侧表）  
| SaleID | ProductID | Quantity | SalesAmount |
|--------|------------|-----------|--------------|
| 1 | A101 | 2 | 200 |
| 2 | A102 | 1 | 100 |
| 3 | A101 | 3 | 300 |

表2：Product（一侧表）  
| ProductID | Category | Price |
|------------|-----------|-------|
| A101 | Laptop | 100 |
| A102 | Mouse  | 100 |

建立关系：  
```
Product[ProductID] (一) —— Sales[ProductID] (多)
```

---

在 Product 表中新建一列：
```DAX
Sales Count = COUNTROWS(RELATEDTABLE(Sales))
```

---

结果：
| ProductID | Category | Price | Sales Count |
|------------|-----------|--------|--------------|
| A101 | Laptop | 100 | 2 |
| A102 | Mouse | 100 | 1 |

---

### 💡 解释
- 对于 `Product[A101]`，`RELATEDTABLE(Sales)` 返回 Sales 表中所有 ProductID=A101 的行（共2行）。  
  → `COUNTROWS()` 计算得到 2。  
- 对于 `Product[A102]`，只找到 1 行。  
- 因此，`RELATEDTABLE()` 返回的是一个“相关行的表”，常用于配合聚合函数（如 `COUNTROWS`、`SUMX`、`AVERAGEX` 等）进行计算。  

---

### 🧠 小结
| 函数 | 方向 | 返回类型 | 常见用途 |
|------|-------|-----------|-----------|
| `RELATED()` | 一 → 多 | 单个值 | 从维度表取值到事实表 |
| `RELATEDTABLE()` | 多 → 一 | 表 | 从事实表聚合到维度表 |


