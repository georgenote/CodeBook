# 01. 删、改、增

## 001. 索引

| Command | Introduction |
| -- | -- |
| `ALTER TABLE table_name ADD PRIMARY KEY ( column )` | 添加主键索引 |
| `ALTER TABLE table_name ADD UNIQUE ( column )` | 添加唯一索引 |
| `ALTER TABLE table_name ADD INDEX index_name ( column )` | 添加普通索引 |
| `ALTER TABLE table_name ADD FULLTEXT ( column )` | 添加全文索引 |
| `ALTER TABLE table_name ADD INDEX index_name ( column1, column2, column3 )` | 添加多列索引 |
