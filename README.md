# Домашнее задание к занятию 12.5. «Индексы» - Паромов Роман

### Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

```
SELECT SUM(DATA_LENGTH) as total_data, SUM(INDEX_LENGTH) as total_index, SUM(INDEX_LENGTH)/SUM(DATA_LENGTH)*100 as procent
from information_schema.TABLES
```
### Задание 2

Выполните explain analyze следующего запроса:
```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места:
  * Очень много ненужных сравней таблиц через WHERE.
  
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

```
SELECT concat(c.last_name, ' ', c.first_name), SUM(p.amount)
from customer c
INNER JOIN payment p ON p.customer_id = c.customer_id 
INNER JOIN rental r ON r.rental_date  = p.payment_date
where date(p.payment_date) = '2005-07-30' and date(p.payment_date) = date(r.rental_date)
GROUP BY c.customer_id, concat(c.last_name, ' ', c.first_name)
```

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*

Самостоятельно изучите, какие типы индексов используются в PostgreSQL. Перечислите те индексы, которые используются в PostgreSQL, а в MySQL — нет.

*Приведите ответ в свободной форме.
