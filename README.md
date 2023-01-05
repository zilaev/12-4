# Домашнее задание к занятию "`SQL. Часть 2`" - `Зилаев Денис`



### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
select s.store_id, count(c.customer_id) as quantity_cust, st.first_name, st.last_name, c2.city 
from store s 
join customer c on s.store_id = c.store_id 
join address a on s.address_id = a.address_id
join city c2 on a.city_id = c2.city_id 
join staff st on s.manager_staff_id = st.staff_id 
GROUP by s.store_id, st.first_name, st.last_name, c2.city
having count(c.customer_id) > 300
```

![задание 1](https://github.com/zilaev/12-4/blob/main/1.png)
....

---

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

`Приведите ответ в свободной форме........`

```
select COUNT(title)
from film
where length > (
select avg(`length`) from film)
```
![задание 2](https://github.com/zilaev/12-4/blob/main/2.png)
---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```
select month(date(payment_date)), sum(amount), count(rental_id)
from payment p 
group by month(date(payment_date))
order by sum(amount) desc
limit 1
```
![задание 3](https://github.com/zilaev/12-4/blob/main/3.png)
---
## Дополнительные задания (со звездочкой*)

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```
select staff_id, count(rental_id),
CASE 
	when count(rental_id) > 8000
	then "yes"
	else "no"
END
as "ПРЕМИЯ"
from rental
group by staff_id 
```
![задание 4](https://github.com/zilaev/12-4/blob/main/4.png)

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

```
select f.film_id, f.title, count(r.rental_id)
from film f 
join inventory i using(film_id)
join rental r on r.inventory_id = i.inventory_id 
group by f.film_id 
having count(r.rental_id) = 0
```
![задание 5](https://github.com/zilaev/12-4/blob/main/5.png)
