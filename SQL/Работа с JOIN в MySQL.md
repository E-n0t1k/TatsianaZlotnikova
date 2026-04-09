# Работа с JOIN в MySQL

## Запросы с JOIN

| Задание | Запрос |
|:--------|:-------|
| Выведите логин вашего пользователя, номера его заказов и их стоимость (таблицы users и orders) | SELECT u.login, o.order_id, o.total <br> FROM users u <br> JOIN orders o ON u.user_id = o.user_id <br> WHERE u.user_id = 6; |
| Выведите номера всех заказов, названия товаров в них и их количество (таблицы order_items и products) | SELECT oi.order_id, p.name, oi.quantity <br> FROM order_items oi <br> JOIN products p ON oi.product_id = p.product_id; |
| Выведите логины всех пользователей и номера заказов, вне зависимости от того, есть ли у них заказ или нет (таблицы users и orders) | SELECT u.login, o.order_id <br> FROM users u <br> LEFT JOIN orders o ON u.user_id = o.user_id; |
| Выведите номера оплаченных заказов и название всех товаров, вне зависимоcти от того, упоминаются ли товары в оплаченных заказах (таблицы products и order_items_paid) | SELECT oip.order_id, p.name <br> FROM order_items_paid oip <br> RIGHT JOIN products p ON oip.product_id = p.product_id <br> ORDER BY oip.order_id; |
| Используя вложенный запрос выведите названия и стоимость товаров, у которых стоимость товара больше, чем стоимость товара "Samsung Active 5" из таблицы products | SELECT name, price <br> FROM products <br> WHERE price > (SELECT price FROM products WHERE name = 'Samsung Active 5'); |

---

## Задача с SQL-academy

| Задание | Решение |
|:--------|:--------|
| Сортировка пассажиров по количеству полетов<br><br>Вывести идентификатор, имя и количество перелётов каждого пассажира, совершившего хотя бы 1 полёт. Результат отсортировать по количеству перелётов (по убыванию) и имени (по возрастанию).<br><br>Используйте конструкцию "as count" для агрегатной функции подсчета количества перелетов. Это необходимо для корректной проверки.<br><br>Поля в результирующей таблице:<br>id<br>name<br>count | SELECT name, count(trip) AS count <br> FROM Passenger <br> INNER JOIN Pass_in_trip <br> ON Pass_in_trip.passenger = Passenger.id <br> GROUP BY name <br> ORDER BY count DESC, name ASC |