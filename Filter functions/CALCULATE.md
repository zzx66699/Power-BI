## 🧩 CALCULATE
### 📘 语法
```DAX
CALCULATE(<expression>, <filter1>[, <filter2>], ...)
```
- `CALCULATE()` 是 DAX 中最核心的函数，用于**在修改筛选上下文（Filter Context）后重新计算表达式**。  
- 它会在执行 `<expression>`（度量或聚合）之前：
  1. **评估现有筛选上下文；**  
  2. **应用或覆盖传入的筛选条件（filters）；**  
  3. 在新的上下文下计算 `<expression>`。  
- 通过 `CALCULATE()`，我们可以改变公式计算的范围或条件（例如忽略某些切片器或添加新的筛选逻辑）。  
- 常用于：
  - 按特定条件计算聚合（如“仅计算 Laptop 类别销售额”）；  
  - 创建动态百分比、同比环比等度量；  
  - 与 `ALL()`、`FILTER()`、`VALUES()` 等函数结合控制上下文。

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

#### 示例 1：计算 Laptop 类别的总销售额
```DAX
Laptop Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    Sales[Category] = "Laptop"
)
```

结果：
| Metric | Value |
|---------|--------|
| Laptop Sales | 500 |

> 说明：即使当前报表筛选了其他类别，`CALCULATE()` 仍会在指定筛选条件下重新求和。  

---

#### 示例 2：计算所有类别的总销售额（忽略筛选）
```DAX
All Category Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    ALL(Sales[Category])
)
```

结果：
| Metric | Value |
|---------|--------|
| All Category Sales | 1000 |

> `ALL()` 移除了 Category 的筛选，因此始终返回全表总额。  

---

#### 示例 3：结合 FILTER() 使用
```DAX
High Sales =
CALCULATE(
    SUM(Sales[SalesAmount]),
    FILTER(Sales, Sales[SalesAmount] > 200)
)
```

结果：
| Metric | Value |
|---------|--------|
| High Sales | 700 |

> `FILTER()` 创建一个满足条件的虚拟表，`CALCULATE()` 在该表上下文中重新计算求和。  

---

### 💬 说明
- `CALCULATE()` 是唯一可以**修改筛选上下文**的函数。  
- 在计算列中使用时，会自动在 row context 与 filter context 之间切换（Context Transition）。  
- 若无筛选条件，仅使用 `CALCULATE(<expression>)`，结果等同于原表达式。  
- 常见组合：
  - `CALCULATE(SUM(...), ALL(...))` → 忽略筛选；
  - `CALCULATE(SUM(...), FILTER(...))` → 条件筛选；
  - `CALCULATE([Measure], VALUES(...))` → 按当前维度重新计算。

---
