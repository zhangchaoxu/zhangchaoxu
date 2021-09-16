# MySQL Cheat Sheet

### 复制表结构
```
CREATE TABLE IF NOT EXISTS db1.a LIKE db2.a
```

### 复制表数据(结构一致)
```
NSERT INTO {sourceDbName.sourceTableName} SELECT * FROM {targetDbName.targetTableName}
```

### 复制表数据(结构不一致)
```
NSERT INTO {sourceDbName.sourceTableName} (字段1,字段2) SELECT 字段1,字段2 FROM {targetDbName.targetTableName}
```

