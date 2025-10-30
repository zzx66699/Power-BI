- Time intelligence functions（时间智能函数）— 处理时间序列范围、同比、环比等计算
  - 关键：它们利用和修改现有的日期筛选上下文，是进行时间比较和累计计算的核心。
  - 时间智能函数本质上是设计用来在 `CALCULATE` 函数内部充当筛选器参数的。
    - 利用 (Utilize) 现有的上下文： 它们首先读取当前计算环境中已有的日期筛选上下文。
    - 修改 (Modify) 现有上下文： 然后，它们根据函数本身的逻辑（例如，平移一年、累计到年初等），生成一个新的日期表来覆盖或替换原有的日期筛选器。
      
Aggregation functions（聚合函数）— 用于计算一个列或表中的总计、平均、最大、最小等。 

Date and time functions（日期与时间函数）— 用于基于日期/时间字段进行计算。 

Filter functions（筛选函数）— 用于返回特定数据、筛选上下文、关系导航。 
Microsoft Learn

Financial functions（财务函数）— 用于执行财务计算，比如现值、收益率等。 

Info functions（信息函数）— 返回模型元数据（例如表、列、关系等）。 

Information functions（类型/信息检查函数）— 检查值或类型是否符合预期，如 ISBLANK()、ISTEXT() 等。 

Logical functions（逻辑函数）— 用于逻辑判断、条件判断，如 IF()、AND()、OR() 等。 
Microsoft Learn

Math and Trig functions（数学与三角函数）— 用于数值计算/算术/三角运算，如 ROUND()、MOD()、DIVIDE() 等。 
Microsoft Learn

Other functions（其他函数）— 执行一些特殊操作，不归入上述类别。 
Microsoft Learn

Parent and Child functions（父子/层次结构函数）— 用于处理父子关系或层次结构数据。 
Microsoft Learn

Relationship functions（关系函数）— 用于管理或使用表间关系，例如 RELATED()、RELATEDTABLE()。 

Statistical functions（统计函数）— 用于统计分布、概率、方差、标准差等。 

Table manipulation functions（表操作函数）— 返回表或对表进行操作（如 SUMMARIZE()、ADD​​COLUMNS() 等）。 
Microsoft Learn


