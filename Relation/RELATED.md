## 🧩 RELATED
### 📘 语法
```DAX
RELATED(<columnName>)
```
如果两个表有relationship，RELATED是根据（一）表里的Relationship Key关系键进行查找，并在（多）表里返回（一）表<columnName>的列。 
### 📊 示例
表1：Sales  
| SaleID | ProductID | Quantity | SalesAmount |
|--------|------------|-----------|--------------|
| 1 | A101 | 2 | 200 |
| 2 | A102 | 1 | 100 |
| 3 | A103 | 1 | 100 |

表2：Product  
| ProductID	|Category	|Price|
|--------|-----------|-----|
|A101|Laptop|	100|
|A102|	Mouse|	100|

建立关系：  
```
Product[ProductID] (一) —— (多) Sales[ProductID]
```

在 Sales 表中新建一列： 
```DAX
Category = RELATED(Product[Category])
```

结果：
| SaleID | ProductID | Quantity | SalesAmount |Category|
|--------|------------|-----------|--------------|-----|
| 1 | A101 | 2 | 200 | Laptop|
| 2 | A102 | 1 | 100 |Mouse|
| 3 | A103 | 1 | 100 |Mouse|


