## 🧩 RELATEDTABLE
### 📘 语法
```DAX
RELATEDTABLE(<tableName>)
```
如果两个表有 relationship，`RELATEDTABLE` 会根据（多）表里的 Relationship Key 关系键进行查找，  
并在（⼀）表中返回与当前行相关的（多）表记录组成的**表**，不是单个值。  

问题是，计算列只能保存单个标量值（scalar value），例如数字、字符串、日期等；  
而RELATEDTABLE() 返回的是整张表（哪怕是一行，也依然是“表”类型）。  

❌ 如果你在计算列中写
```
Sales = RELATEDTABLE(Sales)
```
Power BI 会直接报错：“The expression refers to multiple columns. Multiple columns cannot be converted to a scalar value.”  
要让它能输出单个结果，你必须在外层加一个聚合函数或迭代器函数，
| 示例公式                                                | 含义       | 返回类型 |
| --------------------------------------------------- | -------- | ---- |
| `COUNTROWS(RELATEDTABLE(Sales))`                    | 统计相关行数量  | 数值   |
| `SUMX(RELATEDTABLE(Sales), Sales[SalesAmount])`     | 求相关销售额总和 | 数值   |
| `AVERAGEX(RELATEDTABLE(Sales), Sales[SalesAmount])` | 求平均销售额   | 数值   |

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


