## 查询MySQL中的Json字段特定Key的值

- `JSON_EXTRACT()`: 通过json_extract函数,获取到了json对象的特定key的值.

- eg: 

|字段名|字段类型|字段值|
|:---:|:---:|:---:|
|`id`|int|1|
|`labels`|json|`{"payment": ["cpm", "cpt"]}`|

```sql
SELECT
    id,
    JSON_EXTRACT(labels, '$.payment') as payment
FROM
    tables;
```
