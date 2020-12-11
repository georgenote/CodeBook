# 01. 删、改、增

## 001. 建表

### 1. 较快，分区表
```sql
CREATE TABLE `db.table`(
  `index1` int, 	
  `index2` int)
PARTITIONED BY ( `dt` string )
stored as orc
```

### 2. 较慢，按照txt格式写入
```sql
CREATE TABLE `db.table`(
  `index1` int, 	
  `index2` int)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
```

## 002. 删表
| Command | Introduction |
| -- | -- |
| `drop table db.table` | 删表 |
| `truncate table db.table` | 删表中数 |
