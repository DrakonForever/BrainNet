# TRUNCATE
*tags:* #database, #sql, #DBddl 
*description:* удаление всех строк таблицы. **После использования этой команды нельзя выполнить rollback.**

---
**Template:**
>TRUNCATE TABLE `schema.`table_name;

```sql
TRUNCATE TABLE STUDENTS;
```

В отличие от delete эта команда немного другим способом удаляет строки, может удалять любые строки и в ней нельзя делать rollback.