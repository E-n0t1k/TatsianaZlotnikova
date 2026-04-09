# Работа с MongoDB

| Задание | Запрос |
|:--------|:-------|
| Создайте новую базу данных qa_shop | use qa_shop |
| Внутри нее создайте коллекцию card_info | db.createCollection('card_info') |
| Внутри коллекций создайте документы, которые будут повторять информацию из таблицы card_info в MySQL | db.card_info.insertMany([ <br>  { card_id: 1, card_type: "VISA", expiry_month: 12, expiry_year: 2026, cvv: 123, balance: 1846, status: "valid", card_code: "8820354696467284" } <br>]) |
| Выведите все id и card_type со статусами valid | db.card_info.find({ status: "valid" }, { card_id: 1, card_type: 1, _id: 0 }) |
| Выведите все id и card_type, у которых expiry_month больше 6 | db.card_info.find({ expiry_month: { $gt: 6 } }, { card_id: 1, card_type: 1, _id: 0 }) |
| Выведите все id и card_type, у которых expiry_month больше 5 и меньше 11 | db.card_info.find({ expiry_month: { $gt: 5, $lt: 11 } }, { card_id: 1, card_type: 1, _id: 0 }) |
| Посчитайте общее количество карт с card_type = VISA | db.card_info.countDocuments({ card_type: "VISA" }) |
| Выведите всю информацию о карточках, в названиях которых содержится буква r | db.card_info.find({ card_type: { $regex: /r/ } }) |