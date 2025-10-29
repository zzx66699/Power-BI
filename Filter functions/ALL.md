## 🧩 ALL
### 📘 语法
```DAX
ALL(<tableName or columnName>)
```
- `ALL()` 用于**移除筛选上下文（Filter Context）**，返回指定表或列中的所有行。它不会应用任何当前的筛选条件或切片器过滤效果。 
- `ALL()` 仍然会**保留 blank 行**（与 `VALUES()` 相似）。  
- 常用于：
  - 在度量值中计算“总体总计（Grand Total）”  
  - 与 `CALCULATE()` 一起使用，忽略部分或全部筛选  
  - 在百分比或占比计算中，作为分母范围（例如占总销售额百分比）

---

### 📊 示例
表：Sales  
| Product | Category | SalesAmount |
|----------|-----------|--------------|
| A | Laptop | 200 |
| B | Laptop | 300 |
| C | Mouse | 100 |
| D | Mouse | 400 |

---

#### 示例 1：忽略所有筛选，计算总销售额
```DAX
Total Sales (All) = CALCULATE(SUM(Sales[SalesAmount]), ALL(Sales))
```

即使当前报告筛选了 “Category = Laptop”，  
此公式仍会返回整个表的总销售额：  

| Metric | Value |
|---------|--------|
| Total Sales (All) | 1000 |

---

#### 示例 2：计算当前类别占总销售额的百分比
```DAX
Category % of Total =
DIVIDE(
    SUM(Sales[SalesAmount]),
    CALCULATE(SUM(Sales[SalesAmount]), ALL(Sales[Category]))
)
```

结果（在按 Category 分组的可视化中）：

| Category | SalesAmount | % of Total |
|-----------|--------------|-------------|
| Laptop | 500 | 50% |
| Mouse | 500 | 50% |

---
