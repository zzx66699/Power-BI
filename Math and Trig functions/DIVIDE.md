## 🧩 DIVIDE
### 📘 语法
```DAX
DIVIDE(<numerator>, <denominator>[, <alternateResult>])
```
- `DIVIDE()` 用于执行**除法运算**，并在除数为 0 或空值时**安全地返回备用结果**（避免报错）。  
- 其中：
  - `<numerator>` 是分子（要被除的数）；  
  - `<denominator>` 是分母（用来除的数）；  
  - `<alternateResult>` 是可选参数，当分母为 0 或空时返回的值（默认返回 `BLANK()`）。  
- 常用于：
  - 计算百分比、比率、均值等除法类指标；  
  - 避免直接使用 `/` 运算符导致的除零错误；  
  - 构建更健壮的度量公式。

---

### 📊 示例
表：Sales  
| Product | SalesAmount | Quantity |
|----------|--------------|-----------|
| A | 200 | 2 |
| B | 300 | 0 |
| C | 150 | *(blank)* |

---

#### 示例 1：安全除法计算平均单价
```DAX
Avg Price = DIVIDE(Sales[SalesAmount], Sales[Quantity])
```

结果：
| Product | SalesAmount | Quantity | Avg Price |
|----------|--------------|-----------|------------|
| A | 200 | 2 | 100 |
| B | 300 | 0 | BLANK() |
| C | 150 | *(blank)* | BLANK() |

> 当分母为 0 或空值时，`DIVIDE()` 返回 `BLANK()` 而不会报错。  

---

#### 示例 2：指定备用返回值
```DAX
Avg Price (Safe) = DIVIDE(Sales[SalesAmount], Sales[Quantity], 0)
```

结果：
| Product | SalesAmount | Quantity | Avg Price (Safe) |
|----------|--------------|-----------|------------------|
| A | 200 | 2 | 100 |
| B | 300 | 0 | 0 |
| C | 150 | *(blank)* | 0 |

> 指定第三个参数后，分母为 0 时将返回 0。

---

#### 示例 3：计算销售额占总额百分比
```DAX
Sales % of Total =
DIVIDE(
    SUM(Sales[SalesAmount]),
    CALCULATE(SUM(Sales[SalesAmount]), ALL(Sales))
)
```

结果：

| Product | SalesAmount | % of Total |
|----------|--------------|-------------|
| A | 200 | 28.6% |
| B | 300 | 42.9% |
| C | 150 | 28.6% |

---
