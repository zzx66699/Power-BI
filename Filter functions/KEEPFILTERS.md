## 📘 KEEPFILTERS()

### 🧩 语法
```DAX
KEEPFILTERS(<filter>)
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<filter>` | 一个筛选条件，可以是表或布尔表达式，例如 `Sales[Quantity] > 3` 或 `FILTER(Sales, Sales[Amount] > 1000)`。 |

---

### 💡 功能说明
加上 `KEEPFILTERS()` 后：
> **新的筛选条件与原有筛选条件求交集（AND）。**  
> 它是一个罕见的函数。它仅在您需要将 `CALCULATE` 内部的筛选器与外部作用于相同列的筛选器进行 AND逻辑组合时才使用。  
> 在筛选器作用于不同列时，DAX 默认的相交行为已经满足需求，因此不需要 `KEEPFILTERS`。  

---

### 📊 示例 1️⃣ — 无 `KEEPFILTERS()`
```DAX
High Sales :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    Sales[Region] = "Europe"
)
```

如果当前报表里已经选了 **Region = "North America"**，  
这个 `CALCULATE()` 会**忽略当前筛选**，直接返回欧洲销售额。

---

### 📊 示例 2️⃣ — 使用 `KEEPFILTERS()`
```DAX
High Sales (Keep Existing Filters) :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    KEEPFILTERS(Sales[Region] = "Europe")
)
```

👉 逻辑：
- 如果当前报表中筛选了 `Region = "North America"`，  
- 又在公式中加了 `KEEPFILTERS(Sales[Region] = "Europe")`，  
- 那最终结果会取两者交集（即没有行满足条件 → 返回 BLANK()）。

🧠 等价于：
```text
Region = "North America" AND Region = "Europe"
```
→ 无结果。

---

### 📊 示例 3️⃣ — 实际有用的应用场景

#### ✅ 场景：筛选子集（叠加条件）计算「用户选择的颜色」和「高利润颜色」的交集  
假设您的 $\text{Products}$ 表中有 $\text{Color}$ 列。  
用户在切片器中选择了 Color 的 三个值：{'Red', 'Blue', 'Green'\}。  
高利润颜色集合 是：{'Green', 'Yellow', 'Black'\}。  
您想在目前的切片器中计算仅包含 '高利润' 颜色 的销售额。

``` DAX
Sales =
Calculate(
    SUM(Sales[Revenue]),
    KEEPFILTERS(
        Product[Color] IN {'Green', 'Yellow', 'Black'}
    )
)
```

---

> 强制相交 (KEEPFILTERS)： 计算结果基于两个集合的交集，即 $\text{\{'Green'\}}$ 的销售额。
> 这在需要限制用户可见数据，但又不能完全忽略用户当前筛选器的场景中非常有用。

### 📊 示例 5️⃣ — 组合用法（高级）

```DAX
High Value EU Sales :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    KEEPFILTERS(Sales[SalesAmount] > 500),
    KEEPFILTERS(Sales[Region] = "Europe")
)
```

👉 同时叠加两个条件：  
- 保留外部筛选  
- 限制金额 > 500  
- 限制地区 = Europe

---

### ⚙️ 注意事项

1. `KEEPFILTERS()` 只能在 `CALCULATE()` 或 `CALCULATETABLE()` 中使用；
2. 对于 **多列过滤** 或 **复杂逻辑（AND/OR）**，  
   建议搭配 `FILTER()`；
3. 如果没有外部筛选上下文，`KEEPFILTERS()` 与普通过滤效果相同；
4. 在构建动态比例、TopN、KPI 分段时特别有用。

---

### 📘 与其他函数比较

| 函数 | 行为 | 典型用途 |
|------|------|-----------|
| `REMOVEFILTERS()` | 移除所有筛选 | 计算总体占比 |
| `KEEPFILTERS()` | 保留外部筛选并叠加 | 计算子集指标 |
| `ALL()` | 移除筛选并返回整表 | 全局计算 |
| `ALLEXCEPT()` | 移除除特定列以外的筛选 | 层级汇总 |
| `ALLSELECTED()` | 保留报表交互筛选 | 用户交互计算 |

---

### ✅ 一句话总结
> `KEEPFILTERS()` 用于让新的过滤条件 **叠加（AND）** 到现有的筛选上下文，  
> 而不是覆盖它。  
>  
> 典型用途：在已经选定的国家、产品、客户范围内，  
> 再计算子集（例如高价、活跃、特定区间）指标。
