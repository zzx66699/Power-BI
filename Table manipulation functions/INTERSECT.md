## 🧩 INTERSECT()

### 📘 语法
```DAX
INTERSECT(<table1>, <table2>)
```

### 📖 功能
返回两个表的交集，只保留在两个表中都存在的行。

### 📂 分类
Table manipulation functions

### 📊 示例
假设有两个表：

Table1:
| ProductID | CustomerID |
| ---------- | ----------- |
| A101       | C01         |
| A102       | C02         |
| A103       | C03         |

Table2:
| ProductID | CustomerID |
| ---------- | ----------- |
| A102       | C02         |
| A104       | C04         |

公式：
```DAX
INTERSECT(Table1, Table2)
```

结果：
| ProductID | CustomerID |
| ---------- | ----------- |
| A102       | C02         |

### ⚠️ 注意
- 两个表必须有相同的列结构（列名和数据类型相同）。
- 返回的行是两个表中完全相同的整行。
- 属于 Table manipulation functions 类别。

### 💡 类似函数
| 函数 | 说明 |
|------|------|
| UNION() | 并集（去重） |
| EXCEPT() | 差集（在第一个表中但不在第二个表中） |
| INTERSECT() | 交集（同时存在于两个表中） |
