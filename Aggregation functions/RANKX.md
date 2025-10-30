## 🧩 RANKX()

### 📘 语法
```DAX
RANKX(<table>, <expression>[, <value>[, <order>[, <ties>]]])
```

---

### 📖 说明
- `RANKX()` 用于**根据指定表达式的结果**，为表中的每一行生成排名值。  
- 它会对 `<table>` 中的行按 `<expression>` 排序，然后返回当前行的名次。  
- 常用于按 **销售额、利润、数量等** 进行排名。

---

### ⚙️ 参数
| 参数 | 描述 |
| ---- | ---- |
| `<table>` | 要进行排名的表或表表达式（例如 `VALUES(Sales[Product])`）。 |
| `<expression>` | 用于排名的度量或表达式（例如 `SUM(Sales[Revenue])`）。 |
| `<value>` | （可选）要计算排名的当前行的值，一般省略。 |
| `<order>` | （可选）`ASC` 升序 或 `DESC` 降序，默认是 `DESC`。 |
| `<ties>` | （可选）如何处理并列：`Skip`（跳过排名）或 `Dense`（不跳过）。默认是 `Skip`。 |

---

### 💡 示例
表：`Sales`

| Product | Revenue |
| -------- | -------- |
| A | 300 |
| B | 500 |
| C | 400 |

---

### ✅ 计算每个产品的销售额排名
```DAX
RANKX(
    ALL(Sales[Product]),
    SUM(Sales[Revenue]),
    ,
    DESC,
    Dense
)
```

---

### 📈 结果
| Product | Revenue | Rank |
| -------- | -------- | ----- |
| B | 500 | 1 |
| C | 400 | 2 |
| A | 300 | 3 |

---

### 🧠 小结
- `ALL()` 解除筛选上下文，使排名在全表范围内计算。  
- 改为 `RANKX(FILTER(...))` 可在特定分类内排名。  
- `Dense` 模式不会跳过名次；`Skip` 模式会。

---

