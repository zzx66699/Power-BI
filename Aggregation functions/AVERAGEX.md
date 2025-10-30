## 🧩 AVERAGEX()

### 📘 语法
```DAX
AVERAGEX(<table>, <expression>)
```

### 📖 说明
- `AVERAGEX()` 会**遍历表中的每一行**，对每行计算 `<expression>`，  
  然后返回这些结果的**平均值**。
- 属于 **迭代函数 (X 函数)**，会逐行评估表达式。

---

### ⚙️ 参数
| 参数 | 描述 |
| ---- | ---- |
| `<table>` | 要遍历的表或表表达式。 |
| `<expression>` | 针对表中每一行计算的表达式。 |

---

### 📊 示例 1️⃣
表：`Sales`

| Product | Quantity | Price |
| -------- | -------- | ------ |
| A | 2 | 10 |
| B | 3 | 20 |
| C | 5 | 30 |

```DAX
AVERAGEX(
    Sales,
    Sales[Quantity] * Sales[Price]
)
```

📊 计算过程：
- A → 2 * 10 = 20  
- B → 3 * 20 = 60  
- C → 5 * 30 = 150  

平均值 = (20 + 60 + 150) / 3 = **76.67**

---
### 📊 示例 2️⃣
If your table `Sales` has multiple line items per order (e.g., each product in the same order is a separate row),  
then calculating `AVERAGEX(Sales, Sales[Revenue])` gives you **average per line item**, **not per order**.

To get **average per order**, you need to:
1. First compute **total revenue per order**.
2. Then compute the **average** of those totals.

```
AveragePerOrder =
AVERAGEX(
  VALUE(
    Sales[OrderNumber]
  ),
  CALCULATE(
    SUM(
      Sales[Revenue]
    )
  )
)  
```  
