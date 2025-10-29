## 🧩 DISTINCT
### 📘 语法
```DAX
DISTINCT(<columnName or table>)
```
`DISTINCT()` 用于返回指定列或表中**唯一值（不重复项）**的表。  
换句话说，它会去除重复行，只保留每个唯一值各一条记录。  

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

#### 示例 1：返回唯一的 Customer 列表
```DAX
Unique Customers = DISTINCT(Sales[Customer])
```

结果（表）：
| Customer |
|-----------|
| John |
| Mary |
| Alex |

---

#### 示例 2：统计唯一客户数量
```DAX
Customer Count = COUNTROWS(DISTINCT(Sales[Customer]))
```

结果：
| Metric |Value |
|------|----|
| Customer Count | 3 |

---

### 💡 解释
- `DISTINCT()` 返回的是**表类型**结果，而非单个值。  
- 如果在度量中使用它，一般要与 `COUNTROWS()`、`SUMX()` 等迭代器函数搭配。  
- `DISTINCT()` 会自动忽略空值（blank），除非你用 `DISTINCTCOUNT()`，它会计算空值。  

