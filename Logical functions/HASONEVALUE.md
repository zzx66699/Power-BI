## 🧩 HASONEVALUE()

### 📘 语法
```DAX
HASONEVALUE(<columnName>)
```

---

### 📖 说明
- `HASONEVALUE()` 用于检测当前筛选上下文中，  
  指定列 `<columnName>` 是否只有 **一个唯一值**。  
- 如果当前上下文中存在且仅存在一个值，则返回 `TRUE`；  
  否则返回 `FALSE`（例如有多个值或没有值）。


---

### ⚙️ 参数
| 参数 | 描述 |
| ---- | ---- |
| `<columnName>` | 要检测的列。 |

---

### 💡 使用场景
`HASONEVALUE()` 常用于：
1. **动态切换显示结果**（例如：在选中单个产品时显示产品详情，否则显示总计）  
2. **避免上下文歧义错误**（例如：`SELECTEDVALUE()` 出错时的逻辑保护）  

---

### 📊 示例表：`Sales`

| Product | Revenue |
| -------- | -------- |
| A | 100 |
| B | 200 |
| C | 300 |

---

### ✅ 示例 1：判断是否选中了单个产品
```DAX
IsSingleProduct =
HASONEVALUE(Sales[Product])
```

| Context | Result |
| -------- | ------- |
| 仅筛选 Product = "A" | TRUE |
| 筛选多个产品 | FALSE |
| 无筛选（总计行） | FALSE |

---

### ✅ 示例 2：在度量中使用
```DAX
Display Revenue :=
IF(
    HASONEVALUE(Sales[Product]),
    SUM(Sales[Revenue]),
    SUM(Sales[Revenue]) / COUNTROWS(VALUES(Sales[Product]))  //这里写COUNTROWS(VALUES())是因为VALUES包含空行，而COUNTROWS也计算空行。如果写DISTINCTCOUNT, 就忽略了空行。
)
```

📖 解释：
- 若当前筛选上下文只有一个产品 → 显示该产品的销售额。  
- 若选中多个产品（或总计行） → 显示平均销售额。

