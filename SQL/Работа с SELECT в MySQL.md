# Работа с SELECT в MySQL

| Задание | Запрос |
|:--------|:-------|
| Выведите информацию обо всех продуктах | SELECT * FROM products; |
| Выведите информацию обо всех продуктах, произведенных Apple в категории Phones | SELECT * <br> FROM products <br> WHERE category='Phones' AND manufacturer='Apple'; |
| Выведите названия продуктов и их стоимость, при условии что в названии содержатся буквы sa в любом месте | SELECT name, price <br> FROM products <br> WHERE name like '%sa%'; |
| Выведите названия продуктов и их стоимость, при условии того, что цена находится в диапазоне от 100 до 1000 долларов | SELECT name, price <br> FROM products <br> WHERE price between 100 AND 1000; |
| Посчитайте сумму всех товаров, произведенных компанией Samsung. Название таблицы в результате запроса должно быть SAMSUNG TOTAL PRICE | SELECT sum(price) AS 'SAMSUNG TOTAL PRICE' <br> FROM products <br> WHERE manufacturer = 'Samsung'; |
| Выведите название всех товаров и их стоимость по убыванию | SELECT name, price <br> FROM products <br> ORDER BY price DESC; |
| Выведите названия всех производителей при условии, чтобы они не повторялись | SELECT DISTINCT manufacturer <br> FROM products; |
| Выведите названия первых двух категорий продуктов, чтобы они не повторялись | SELECT DISTINCT category <br> FROM products <br> LIMIT 2; |
| Выведите названия продуктов при условии, что они состоят из 12 символов и их названия начинаются с A | SELECT name <br> FROM products <br> WHERE length(name) = 12 and name LIKE "A%" |
| Посчитайте среднюю цену всех продуктов. Название таблицы в результате запроса должно быть PRODUCTS AVG PRICE | SELECT AVG(price) AS 'PRODUCTS AVG PRICE' <br> FROM products; |
| Используя оператор IN, выведите названия и описание продуктов, у которых производитель Samsung и Huawei | SELECT name, description <br> FROM products <br> WHERE manufacturer IN ('Samsung', 'Huawei') |
| Используя оператор UNION, выведите информацию о названии товаров из таблицы products и номера заказов из таблицы orders | SELECT name <br> FROM products <br> UNION <br> SELECT order_id <br> FROM orders |
| Используя оператор HAVING, посчитайте количество товаров в каждой категории, оставив только те категории, в которых количество товаров больше 15 | SELECT count(category), category <br> FROM products <br> GROUP BY category <br> HAVING count(category) > 15 |
| Используя оператор CASE опишите следующую логику:<br>Выведите компанию, категорию, стоимость и название товара, а также следующий текстовое сообщение:<br><br>Если компания Apple, то в консоли должно вывестись "Это продукт компании Apple".<br><br>Если компания Samsung, то в консоли должно вывестись "Это продукт компании Samsung".<br><br>Если компания Huawei, то в консоли должно вывестись "Это продукт компании  Huawei".<br><br>Если компания Xiaomi, то в консоли должно вывестись "Это продукт компании Xiaomi". | SELECT manufacturer, category, price, name, <br> CASE <br>     WHEN manufacturer = 'Apple' THEN "Это продукт компании Apple" <br>     WHEN manufacturer = 'Samsung' THEN "Это продукт компании Samsung" <br>     WHEN manufacturer = 'Huawei' THEN "Это продукт компании Huawei" <br>     WHEN manufacturer = 'Xiaomi' THEN "Это продукт компании Xiaomi" <br>     ELSE "Это продукт другой компании" <br> END AS text <br> FROM products; |