==LPAD(s,n,p)== - добавление символов слева. n - конечная длина текста, p - текст для заполнения.
	
*Если конечная длина меньше, чем начальная (s), то длина будет урезаться с конца!!!*
	
***Примеры:***
```sql
SQL> select LPAD('Zaur', 7, '*') from dual;

LPAD('Z
-------
***Zaur
```
---
*tags:* #database, #sql, #sqlFunction #character_func #single-row_func 