## 🧩 CALCULATETABLE()
### 📘 语法
```DAX
CALCULATETABLE(<table>, <filter1>, <filter2>, ...)
```
- `CALCULATETABLE()` 用于在 修改筛选上下文（filter context） 后返回一个表格（table）。
- 函数常用于创建动态筛选后的中间表，用于后续计算或作为迭代函数的输入。
---

### 📊 示例
表：Sales  
| SaleID | ProductID | Quantity | SalesAmount |
| ------ | --------- | -------- | ----------- |
| 1      | A101      | 2        | 200         |
| 2      | A102      | 5        | 300         |
| 3      | A101      | 1        | 100         |
| 4      | A103      | 4        | 400         |

---

#### 示例 1：返回筛选后的表
```DAX
High Quantity Sales =
CALCULATETABLE(
    Sales,
    Sales[Quantity] > 3
)
```

结果：
| SaleID | ProductID | Quantity | SalesAmount |
| ------ | --------- | -------- | ----------- |
| 2      | A102      | 5        | 300         |
| 4      | A103      | 4        | 400         |


---
#### 示例 2：在 SUMX() 中限定迭代范围
```DAX
High Quantity Sales Total =
SUMX(
    CALCULATETABLE(
        Sales,
        Sales[Quantity] > 3
    ),
    Sales[SalesAmount]
)
```

结果：
| Metric                    | Value |
| ------------------------- | ----- |
| High Quantity Sales Total | 700   |
