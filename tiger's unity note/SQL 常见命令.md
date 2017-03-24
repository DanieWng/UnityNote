# SQL 常见命令 

*	**查询所有的重复字段**

```sql
Select ID, KEY_C, CHARACTER, Count(*) From KeyCharacterListTable Group By CHARACTER Having Count(CHARACTER) > 1
```



