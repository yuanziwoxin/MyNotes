---
title: MySQL之添加和删除外键约束
---
# MySQL之添加和删除外键约束

## 1.  创建表时添加外键约束
```sql
CREATE TABLE Orders
(
Id_O int NOT NULL,
OrderNo int NOT NULL,
Id_P int,
PRIMARY KEY (Id_O),
CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P)
REFERENCES Persons(Id_P)
)
-- 注：
-- （1）一共两个表（Persons表和Orders表），Id_P是Persons表的主键；
-- （2）fk_PerOrders表示创建的外键约束的名称
```
## 2. 创建表后以修改表的形式添加外键约束
```sql
alter table table_name add constraint fk_name foreign key (fk_field_name) references refer_table_name(primary_key_name) on delete restrict on update restrict;
-- 注：
-- table_name：表名
-- fk_name:设置的外键约束的名称
-- fk_field_name:外键字段名
-- refer_table_name:参照表的名称
-- primary_key_name:对应的参照表的字段名
```

On Delete和On Update都有Restrict，No Action, Cascade,Set Null属性。现在分别对他们的属性含义做个解释。

- **（1）ON DELETE**
 - **restrict(约束)**:当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除。

 - **no action**:意思同restrict.即如果存在从数据，不允许删除主数据。

 - **cascade(级联)**:当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则也删除外键在子表（即包含外键的表）中的记录。

 - **set null**:当在父表（即外键的来源表）中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（不过这就要求该外键允许取null）

- **（2）ON UPDATE**
 - **restrict(约束)**:当在父表（即外键的来源表）中更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许更新。

 - **no action**:意思同restrict.

 - **cascade(级联)**:当在父表（即外键的来源表）中更新对应记录时，首先检查该记录是否有对应外键，如果有则也更新外键在子表（即包含外键的表）中的记录。

 - **set null**:当在父表（即外键的来源表）中更新对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（不过这就要求该外键允许取null）。

**注：**
NO ACTION和RESTRICT的区别：只有在及个别的情况下会导致区别，**前者**是在**其他约束的动作之后**执行，后者**具有最高的优先权执行**。

## 3. 删除外键约束
```sql
ALTER TABLE Orders
DROP FOREIGN KEY fk_PerOrders;
-- fk_PerOrders表示外键约束名称
```



参考文章：

[1. MySQL外键约束On Delete和On Update的使用](https://blog.csdn.net/dingding_12345/article/details/47905715)

[2. SQL FOREIGN KEY 约束](http://www.w3school.com.cn/sql/sql_foreignkey.asp)
