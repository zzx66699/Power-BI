## 📘 REMOVEFILTERS()

### 🧩 语法
```DAX
REMOVEFILTERS([<table_or_column>])
```

### 📖 参数说明
| 参数 | 说明 |
|------|------|
| `<table_or_column>` *(可选)* | 指定要移除筛选的表或列。如果省略，则移除整个模型的筛选。 |

---

### 💡 功能说明
`REMOVEFILTERS()` 会：
1. 识别当前上下文中某个表或列被应用的筛选条件；
2. 移除这些筛选；
3. 返回一个“未被筛选”的表供后续计算使用。
4. 它是现代 DAX 中推荐使用的语法（取代早期的 `ALL()` 用于“移除筛选”的场景）。
---

### 📊 示例 1️⃣ — 忽略某列筛选
```DAX
Total Sales All Customers :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    REMOVEFILTERS(Sales[CustomerKey])
)
```

👉 含义：
- 在当前报表中，可能用户筛选了某个客户；
- 但这个度量强制忽略客户筛选；
- 返回**所有客户的总销售额**。

---

### 📊 示例 2️⃣ — 忽略整张表的筛选
```DAX
Total Sales All Dates :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    REMOVEFILTERS('Date')
)
```

👉 含义：
- 移除 `Date` 表上的时间筛选；
- 计算全时期的销售额。

---

### 📊 示例 3️⃣ — 同时移除多张表
```DAX
Total Sales (Ignore Date & Customer) :=
CALCULATE(
    SUM(Sales[SalesAmount]),
    REMOVEFILTERS('Date'),
    REMOVEFILTERS(Sales[CustomerKey])
)
```

👉 含义：
无论报表中选择了什么日期或客户，  
都返回全表的销售额。

---

### 📊 示例 4️⃣ — 计算百分比
假设有表
| Group         | Country | Region     | Revenue |
| ------------- | ------- | ---------- | ------- |
| Europe        | France  | France     | 100     |
| Europe        | Germany | Germany    | 120     |
| Europe        | UK      | UK         | 80      |
| North America | Canada  | Canada     | 150     |
| North America | USA     | Central    | 70      |
| North America | USA     | North East | 60      |
| North America | USA     | North West | 50      |
| North America | USA     | South East | 40      |
| North America | USA     | South West | 30      |

#### 🧩 Step 0 — 基础度量

```DAX
Total Revenue :=
SUM(Geography[Revenue])
```

---

#### 🧮 ① Region Revenue % of Total

```DAX
Region % of Total :=
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        REMOVEFILTERS(Geography)
    )
)
```

📘 说明：
- 分子：当前行的 Region Revenue；
- 分母：移除所有筛选后的全表总 Revenue。

---

#### 🧮 ② Region Revenue % of Country

```DAX
Region % of Country :=
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        REMOVEFILTERS(Geography[Region])   // 移除 Region 层级，仅保留 Country
    )
)
```

📘 说明：
- 保留当前 Country 的上下文；
- 忽略 Region 级筛选；
- 计算该国家下所有 Region 的总和。

示例（USA）：

| Country | Region | Revenue | % of Country |
|----------|----------|----------|---------------|
| USA | Central | 70 | 70 / (70+60+50+40+30) = 24.1% |
| USA | North East | 60 | 20.7% |
| USA | South West | 30 | 10.3% |

---

#### 🧮 ③ Region Revenue % of Group

```DAX
Region % of Group :=
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        REMOVEFILTERS(Geography[Region], Geography[Country])  // 保留 Group
    )
)
```

📘 说明：
- 移除 Region、Country 两级筛选；
- 仅保留 Group 层级上下文；
- 用 Group 总 Revenue 作为分母。

---

#### ⚙️ 对比：REMOVEFILTERS 层级控制

| 计算目标 | 移除筛选 | 保留筛选 |
|-----------|-----------|-----------|
| % of Total | `REMOVEFILTERS(Geography)` | 无 |
| % of Country | `REMOVEFILTERS(Geography[Region])` | Country |
| % of Group | `REMOVEFILTERS(Geography[Region], Geography[Country])` | Group |

---

#### ✅ 最终结果示例

| Group | Country | Region | Revenue | % Total | % Country | % Group |
|--------|----------|----------|----------|----------|-----------|----------|
| Europe | France | France | 100 | 10.7% | 100% | 33.3% |
| Europe | Germany | Germany | 120 | 12.8% | 100% | 40.0% |
| Europe | UK | UK | 80 | 8.5% | 100% | 26.7% |
| North America | Canada | Canada | 150 | 16.0% | 100% | 40.5% |
| North America | USA | Central | 70 | 7.5% | 24.1% | 18.9% |
| North America | USA | North East | 60 | 6.4% | 20.7% | 16.2% |
| North America | USA | North West | 50 | 5.3% | 17.2% | 13.5% |
| North America | USA | South East | 40 | 4.3% | 13.8% | 10.8% |
| North America | USA | South West | 30 | 3.2% | 10.3% | 8.1% |

