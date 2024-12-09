# SQL_Pamyatka
Памятка, с основными командами, и терминами SQL

Уроки GB
урок 17
Агрегирующие функции COUNT количество столбцов:
SELECT count(*) FROM product;

урок 17
Агрегирующие функции COUNT количество с ограничениями:
SELECT count(*) FROM `product` where `product`.`price` < 10000;

урок 17
Агрегирующие функции SUM суммирование чисел, MIN – минимальное значение, MAX — максимальное значение (агрегирующие функции можно писать в одном запросе, результат будет выведен в строчку несколькими значениями):
SELECT sum(price), min(price), max(price) FROM `product`;

урок 19
Агрегирующие ограниченные функции (where не всегда подходит для агрегирующих функций, вместо этого используется hawing)
вместо where sum(`count`) >= 5; --тут ошибка
нужно HAWING SUM(`count`) >=5;

урок 18
Группирование, разбиение на группы Агрегирующих функций(поиск MAX цены и SUMмы заказов будет проводиться для каждого user_name отдельно, и выведено на экран)
SELECT `order`.`user_name`, MAX(price), SUM(`count`), FROM `order`
	INNER JOIN `order_products` ON `order_products`.`order_id` = `order`.`id`
	INNER JOIN `product` ON `product`.`id` = `order_products`.`product_id`
		GROUP BY `order`.`user_name`;

урок 4
Добавление колонки в таблицу (колонка`alias_name` для таблицы `category`, после столбца `discount`):
ALTER TABLE `shop`, `category`
ADD COLUMN `alias_name` VARCHAR(128) NULL AFTER `discount`;

урок 5
Заполнение таблицы (таблицу `category` для БД `shop):
INSERT INTO ‘shop’.’category’ (‘id’,’name’, ‘discount’) VALUES (‘1’,’Женская одежда’, ’5‘);

урок 4
Изменение таблицы (добавление колонки `alias_name` в таблицу `category` для БД `shop`):
ALTER TABLE `shop`. `category`
ADD COLUMN `alias_name` VARCHAR(128) NULL AFTER `discount`;

урок 5
Изменение таблицы (изменение свойств колонки `id` добавление автоинкремента в таблице `category` для БД `shop`):
ALTER TABLE `shop`. `category`
CHANGE COLUMN `id` `id` INT(11) NOT NULL AUTO_INCREMENT;

урок 8
Изменение содержимого таблицы (изменение имени `Шляпы` на `Головные уборы` в поле `name` в таблице `shop.category`):
UPDATE category SET name = `Головные уборы` WHERE id = 5;

урок 8
Изменение содержимого таблицы (изменение всех значений `0` на `3` в поле `discount` в таблице `shop.category`):
UPDATE category SET discount = 3 WHERE discount = 0;
(НЕЖЕЛАТЕЛЬНО, но в начале необходимо изменить настройки MySQL в панели Edit/Preferences/SQL Editor, убрать галочку в поле ”Safe updates”.Forbid UPDATEs...)

урок 8
Изменение содержимого таблицы (изменение  значений на `3` в поле `discount` в таблице `shop.category` где id = 2 и 5 (сокращенная форма записи)):
UPDATE category SET discount = 3 WHERE id IN (2,5);

урок 19
Индексирование столбца (для ускорения поиска и агрегирующих запросов по этому столбцу больших БД)
ALTER TABLE `shop`.`product`
ADD INDEX `price_index` (`price` ASC);

урок 16
Обьединение двух таблиц:
select * from product_type where id = 1
union
select * from product_type where id = 2;

урок 16
Отображение двух связанных таблиц полностью, даже если есть пустые строки в одной из таблиц (не работает только в MySQL):
SELECT * FROM `order`
FULL OUTER JOIN `order_products` ON `order_products`.`order_id` = `order`.`id`
FULL OUTER JOIN `product` ON `order_products`.`product_id` = `product`. `id`;

(Эквивалент в MySQL)
select * from `order`
	left join order_products on order_products.order_id = `order`.id
    left join product on order_products.product_id = product.id
    
union

select * from `order`	
	inner join order_products on order_products.order_id = `order`.id
    right join product on order_products.product_id = product.id
    where `order`.id is null;
    
урок 4
Отображение списка БД в консоли MySQL:
show databases;

урок 4
Отображение списка таблиц выбранной для работы базы данных:
show tables;

урок 4
Отображение таблицы (`category` в выбранной БД для работы):
show columns category;

урок 4, 6
Отображение всей таблицы (`category` из БД `shop`):
SELECT * FROM shop.category;

урок 7
Отображение всей таблицы с сортировкой по возростанию (по размеру discount таблицы`category` из БД `shop`):
SELECT * FROM shop.category ORDER BY discount;
Или
SELECT * FROM shop.category ORDER BY discount ASC;


урок 7
Отображение всей таблицы с сортировкой по убыванию (по размеру discount таблицы`category` из БД `shop`):
SELECT * FROM shop.category ORDER BY discount DESC;

урок 7
Отображение колонки таблицы (колонки name таблицы `category` из БД `shop`):
SELECT name FROM shop.category;

урок 15
Отображение колонки таблицы с переименованием заголовка (колонки name переименованной в tovar таблицы `category` из БД `shop`):
SELECT name as tovar FROM shop.category;

Урок 7
Отображение несколько первых строк таблицы (2 первые строки таблицы`category` из БД `shop`):
SELECT * FROM shop.category LIMIT 2;

Урок 7
Отображение несколько первых строк таблицы с условием (2 первые строки таблицы`category` из БД `shop` в которых скидка не равна 0):
SELECT * FROM shop.category WHERE discount <> 0 LIMIT 2;

урок 7
Отображение нескольких колонок таблицы (колонки name, discount таблицы `category` из БД `shop`):
SELECT name, discount FROM shop.category;

урок 7
Отображение нескольких колонок таблицы в другом порядке (колонки name, discount таблицы `category` из БД `shop`):
SELECT discount, name FROM shop.category;

урок 15
Отображение нескольких таблиц со связанным значением (3 полные таблицы `product`, `category`, `brand` из БД `shop`. Если в какой-то строке связь отсутствует, то строка не отобразится целиком):
SELECT * FROM `shop`.`product`
	INNER JOIN `shop`.`category` ON `product`.`category_id` = `category`.`id`
	INNER JOIN `shop`.`brand` ON `product`.`brand_id` = `brand`.`id`;

урок 15
Отображение нескольких таблиц со связанным значением и всеми значениями из левой таблицы (2 полные таблицы `product`, `category`, `brand` из БД `shop`. Если в какой-то строке связь отсутствует, то строка не отобразится целиком, но левая таблица отобразится целиком):
SELECT * FROM `shop`.`product`
	LEFT JOIN `shop`.`category` ON `product`.`category_id` = `category`.`id`;

урок 15
Отображение только одной таблицы, где вторая таблица пуста, сокрытие пустых строк второй таблицы (скрывает SELECT category.*) (2 таблицы `product`, `category` из БД `shop`.):
SELECT category.* FROM `shop`.`product`
	LEFT JOIN `shop`.`category` ON `product`.`category_id` = `category`.`id`
	WHERE `product`.`id` is null;

урок 7
Отображение колонки таблицы с уникальными значениями (т.е. без дубликатов) (колонки discount таблицы `category` из БД `shop`):
SELECT DISTINCT discount FROM shop.category;

Урок 7
Отображение с несколькими условиями (`category` из БД `shop`) (порядок WHERE и ORDER BY важен):
SELECT * FROM shop.category WHERE discount <> 0 ORDER BY discount DESC;

урок 6
Отображение части строк таблицы (`category` из БД `shop` где id = 3):
SELECT * FROM shop.category WHERE id = 3;

урок 6
Отображение части строк таблицы (`category` из БД `shop` где скидка не равна 0):
SELECT * FROM shop.category WHERE discount <> 0;

урок 6
Отображение части строк таблицы несколько условий одновременно (`category` из БД `shop):
SELECT * FROM shop.category WHERE (discount > 5) AND (discount < 15);

урок 6
Отображение части строк таблицы несколько условий отдельно (`category` из БД `shop):
SELECT * FROM shop.category WHERE (discount <5) OR (discount >=10);

урок 6
Отображение части строк таблицы (`category` из БД `shop где есть псевдоним):
SELECT * FROM shop.category WHERE alias_name IS NOT NULL;

урок 6
Отображение части строк таблицы отрицание (`category` из БД `shop):
SELECT * FROM shop.category WHERE NOT (discount < 5);

урок 4
Работа с БД (`shop`) в консоли MySQL:
use shop;

урок 10
Согласованность таблиц в БД, создание связи (БД - `shop`, табл `product`, колонка `brand_id`, с колонкой `id` таблицы `brand` в примере) в MySQL:
ALTER TABLE `shop`.`product`
ADD CONSTRAINT `fk_brand_product`
FOREIGN KEY(`brand_id`)
REFERENCES `shop`.`brand`(`id`)
ON DELETE NO ACTION		 — есть варианты RESTRICT, CASCADE, SET NULL, 									NO ACTION
ON UPDATE NO ACTION;

урок 10
Согласованность таблиц, удаление имеющейся согласованности (`fk_brand_product` - ранее созданная согласованность) в MySQL:
ALTER TABLE `shop`.`product`
DROP FOREIGN KEY `fk_brand_product`;

урок 10
Согласованность таблиц, изменение имеющейся согласованности (`fk_brand_product` - ранее созданная согласованность, добавление действия при удалении элемента) в MySQL:
ALTER TABLE `shop`.`product`
DROP FOREIGN KEY `fk_brand_product`;
ALTER TABLE `shop`.`product`
ADD CONSTRAINT `fk_brand_product`
FOREIGN KEY(`brand_id`)
REFERENCES `shop`.`brand`(`id`)
ON DELETE CASCADE
ON UPDATE NO ACTION;

урок 4
Создание Базы банных (`shop` в примере) в MySQL:
CREATE SCHEMA `shop` DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

урок 4
Создание таблицы (`category` для БД `shop`) с атрибутами:
CREATE TABLE `shop`.`category` (
 `id` INT NOT NULL,
 `name` VARCHAR(128) NOT NULL,
 `discount` TINYINT NOT NULL,
PRIMARY KEY(`id`));

урок 20
Транзакции (код, между START TRANSACTION  …  COMMIT гарантированно выполнится в полном объёме, либо весь не выполнится)
--создание демо счета
INSERT INTO `shop`.`user_bank_account` (`id`, `money`, `user_name`) VALUES (`1`, `100`, `Дмитрий`);
INSERT INTO `shop`.`user_bank_account` (`id`, `money`, `user_name`) VALUES (`2`, `200`, `Евгений`);

--сама транзакция
START TRANSACTION;
	update `shop`.`user_bank_account` SET `money` = `money` - 100 WHERE `id` = 1;
	update `shop`.`user_bank_account` SET `money` = `money` + 100 WHERE `id` = 2;
COMMIT;

урок 19
Условие поиска по первой букве (начинаются на букву `В`)
WHERE `user_name` LIKE `В%`;

урок 4
Удаление БД (`shop`):
DROP DATABASE `shop`

урок 4
Удаление таблицы (`category` из БД `shop`):
DROP TABLE `shop`.`category`

урок 8
Удаление строки категории (`Головные уборы` из `shop.category`):
DELETE FROM `shop`.`category` WHERE id = 5;


https://dev.mysql.com/doc/refman/5.7/en/  - официальная документация MySQL

Подсказки из другой книги. «SQL быстрое погружение Уолтер Шилдс». (актуально для SQLite)

Стр 63.
Добавление псевдонима (временного названия столбца, если старое название усложняет восприятие информации. Название существует только в рамках запроса, не меняя базу данных.)
(Псевдонимы обычно связанны с ключевым словом AS, однако в большинстве реализаций РСУБД ключевое слово AS между именем поля и псевдонимом использовать не обязательно.
Одинарные кавычки и квадратные скобки нужны для случаев, когда в результате, в столбце, должно получиться более одного слова, разделенных между собой пробелом.)
(Вывод имен клиентов и их Email и номера телефона из БД `sTunes` из таблицы `customers`, переименовав столбцы)
SELECT
	FirstName AS ‘First Name’,
	LastName AS [Last Name],
	Email AS EMAIL,
	Phone CELL
FROM
	customers;

Стр 67.
Ограничение числа записей при отображении. Условие LIMIT.
(отобразить только 10 записей из БД `sTunes`, таблицы `customers`,)
SELECT
	FirstName AS [First Name],
	LastName AS [Last Name],
	Email AS [EMAIL]
FROM
	customers
ORDER BY
	FirstName ASC,
	[Last Name] DESC
LIMIT 10;

Стр. 76
Вывод диапазона чисел, оператор BETWEEN. (из БД `sTunes`, табл `invoices`)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Вывести количество счетов в диапазоне от 1,98 до 5 долларов.
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE
	Total BETWEEN 1.98 AND 5.00
ORDER BY
	InvoiceDate;

Тоже самое, но при помощи логических операторов сравнения. (Перед AND не забыть снова написать Total)
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE
	Total >= 1.98 AND Total <= 5.00
ORDER BY
	InvoiceDate;

Стр.77
Оператор сравнения IN для поиска совпадающих значений. 
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Возврат только суммы счетов, равных 1,98 или 3,96
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE
	Total IN (1.98, 3.96)
ORDER BY
	InvoiceDate;

П.с. Аналогично сделать с арифметическим оператором не получится, т. к. оператор = одновременно выводит только для одного значения.

Стр.79
Аналогично работает для сравнения текста. (Нужны одинарные кавычки)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: вывести счета в городе Tucson, 'Paris', 'London'
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE
	BillingCity IN ('Tucson', 'Paris', 'London')
ORDER BY
	Total;

Стр.90
Скобки. (Если не вставить скобки, то условие `d%` отработает без учета цены и присуммируется. AND – аналог умножения, OR – сложения. Складывают данные из одного столбца, к ним умножают данные из другого столбца.)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: испольоаение скобок. 
Вывод городов начинающихся на p и d. При этом стоимость должна быть выше 3.00ю
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE
	Total > 3.00 AND (BillingCity LIKE 'p%' OR BillingCity LIKE 'd%')
ORDER BY
	Total;

Стр. 93
Добавление новой категории для отображения в таблице. (CASE должен быть внутри SELECT)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Добавление своей категории в отображенную таблицу.
Цены разбиты на 4 ценовых диапазона. Каждому диапазону присвоено название.
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total,
	CASE
		WHEN Total < 2.00 THEN 'Baseline Purchase'
		WHEN Total BETWEEN 2.00 AND 6.99 THEN 'Low Purchase'
		WHEN Total BETWEEN 7.00 AND 15.00 THEN 'Target Purchase'
		ELSE 'Top Performers'
	END AS PurchaseType
FROM
	invoices
ORDER BY
	BillingCity;

Стр. 99
Отображение двух объединенных таблиц
SELECT
	*
FROM
	invoices
INNER JOIN
	customers
ON
	invoices.CustomerId = customers.CustomerId

Стр.115
Отображение части 3 реляционных таблиц, пример (из БД `sTunes`)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: соединение 3 таблиц, пример
*/

SELECT
	e.FirstName,
	e.LastName,
	e.EmployeeId,
	c.FirstName,
	c.LastName,
	c.SupportRepId,
	i.CustomerId,
	i.Total
FROM
	invoices AS i
INNER JOIN
	customers AS c
ON
	i.CustomerId = c.CustomerId
INNER JOIN
	employees AS e
ON
	c.SupportRepId = e.EmployeeId
ORDER BY
	i.Total DESC
LIMIT 10

Стр. 129
Конкатенация (объединение) строк (из БД `sTunes`)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: обьединение двух слов с добавлением пробела, и вывод результата в отдельный столбец
*/

SELECT
	FirstName,
	LastName,
	FirstName || ' ' || LastName AS [Full name]
FROM
	customers
WHERE
	Country = 'USA'

Стр.132 
Редактирование выведенного текста, вырезание лишних символов с сипользованием SUBSTR(x,y,z) (из БД `sTunes`)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: удаление лишних символов из почтового кода США (нужны только первые 5 цифр)
SUBSTR извлекает из строчки PostalCode 5 символов, начиная с 1.
*/

SELECT
	PostalCode,
	SUBSTR(PostalCode,1,5) AS [Five Digit Postal Code]
FROM
	customers
WHERE
	Country = 'USA'

Стр. 134
Изменение регистра шрифта, верхний и нижний регистры(А так-же, вырезание части символов, оставление только первой заглавной буквы имени.)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Изменение регистра шрифта.
*/

SELECT
	FirstName AS [First Name Unmodified],
	UPPER(FirstName) AS [First Name in UPPERCASE],
	LOWER(FirstName) AS [First Name in lowercase],
	UPPER(FirstName) || ' ' || UPPER(LastName) AS [Full Name in UPPERCASE],
	UPPER(SUBSTR(FirstName, 1, 1)) || ' ' || UPPER(LastName)  AS [Last Name and 1 Initial]
FROM
	customers

Стр. 138
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Пример работы функции преобразования даты и времени STRFTIME()
*/

SELECT
	STRFTIME('The Year is: %Y The Fay is %d The Month is %m', 
	'2011-05-22') AS [Text with Conversion Specifications]

Стр. 139
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Пример работы функции преобразования даты и времени STRFTIME()
Вычисление возраста, с помошью этой функции, и текущей даты.
*/

SELECT
	LastName,
	FirstName,
	STRFTIME('%Y-%m-%d', BirthDate) AS [Birthday No Timecode],
	STRFTIME('%Y-%m-%d', 'now')- STRFTIME('%Y-%m-%d', BirthDate) AS [Age]
FROM
	employees
ORDER BY
	Age

Стр.148
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: испольщование агрегатных функций не в поле SELECT
(т.к. в WHERE нельзя, можно после GROUP BY)
*/

SELECT
	BillingCity,
	AVG(Total)
FROM
	invoices
WHERE
	BillingCity LIKE 'B%'
GROUP BY
	BillingCity
HAVING
	AVG(Total) > 5
ORDER BY
	BillingCity

Стр. 154
Подзапросы(т.к. внутри WHERE нельзя использовать агрегатную функцию, использовали ее внутри подзапроса.)
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Вывод всех счетов, которые меньше средней величины счета 5,65.
С использованием подзапроса.
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE Total < 
(	SELECT 
		AVG(Total)
	FROM
		invoices)
ORDER BY
	Total DESC

Стр. 156
ROUND для округления, пример.
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Вывод всех счетов, которые меньше средней величины счета 5,65.
Плюс, визуальное сравнение средней величины Total.
С использованием подзапроса.
И ROUND для округления до сотой.
*/

SELECT
	BillingCity,
	ROUND(AVG(Total), 2) AS [City Average],
	(SELECT
		ROUND(avg(total), 2)
	FROM
		invoices) AS [Global Average]
FROM
	invoices
GROUP BY
	BillingCity
ORDER BY
	BillingCity

Стр. 156
Пример подзапроса2. Крупный, полноценный подзапрос.
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Поиск продажи, после 2013 года, 
которая превышает максиальную продажу, до 2013 года.
*/

SELECT
	InvoiceDate,
	BillingCity,
	Total
FROM
	invoices
WHERE
	InvoiceDate >= '2013-01-01' AND Total >
	(SELECT
		MAX(Total)
	FROM
		invoices
	WHERE
		InvoiceDate < '2013-01-01')

Стр. 159
Подзапрос, для получения нескольких результатов.
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Получение нескольких результатов, с использованием подзапроса.
ВО внутреннем подзапросе вытаскиваем 3 конкретных даты по 3 конеретным id.
Во внешнем запросе, смотрим, были ли еще покупки за эти 3 дня.
*/

SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity
FROM
	invoices
WHERE
	InvoiceDate IN
	(SELECT
		InvoiceDate
	FROM
		invoices
	WHERE
		InvoiceId IN (251, 252, 255))

Стр.162
Использование DISTINCT в подзапросе и NOT IS
/*
CREATED BY: Raul Ahmedzianov
CREATED ON: 2024.10.03
DESCRIPTION: Отобразить только те треки, которые ниразу не покупались.
Во внутреннем подзапросе отображаются треки, которые покупались.
DISTINCT нужен, чтобы отобразить трек лишь 1 раз, т.к. некоторые треки покупались по несколько раз.
Во внешнем запросе, идет отображение тех треков, которые не вошли в список внутреннего запроса.
*/

SELECT
	TrackId,
	Composer,
	name
FROM
	tracks
WHERE
	TrackId NOT IN
	(SELECT
		DISTINCT TrackId
	FROM
		invoice_items
	ORDER BY
		TrackId)

Стр.165
Представление, пример создания (Заранее подготовленный блок кода, для будущего многократного использования. V_AvgTotal – название представления, которое вы можете выбрать сами, после добавки V_)
CREATE VIEW V_AvgTotal AS
SELECT
	ROUND(AVG(Total), 2) AS [Average Total]
FROM
	invoices

Стр. 167
Представление, пример использования
SELECT
	InvoiceDate,
	BillingAddress,
	BillingCity,
	Total
FROM
	invoices
WHERE
	Total <
	(SELECT
		*
	FROM
		V_AvgTotal)
ORDER BY
	Total DESC

Стр. 171
Представление, пример удаления
DROP VIEW
	V_AvgTotal

Стр.172
Представление, пример использования, внутри SELECT(нужно создать подзапрос и SELECT со звездочкой)
SELECT
	BillingCity,
	AVG(Total) AS [City Average],
	(SELECT
		*
	FROM
	V_GlobalAverage) AS [Global Average]
FROM
	invoices
GROUP BY
	BillingCity
ORDER BY
	BillingCity



Определения основы SQL:

Данные — это часть информации. Данные находятся повсюду и содержатся везде, но на практике термин «данные» относится к информации уже записанной или той, которую можно записать.

Таблица (базовая относительная переменная, связь) — один из самых простых инструментов, используемых для записи и визуализации данных.

Таблица — двухмерная сетка, состоящая из строк и столбцов.

Метаданные — «данные о данных» Данные, описывающие структуру и форматирование самих данных. (Например, данные о типе данных внутри столбца: текст, цифры, дата, ограничение чиста до целой или до тысячной, и так далее.)

«База данных» - совокупность данных упорядоченная для упрощения и скорости поиска и извлечения с помощью компьютера.

Строки в таблице называются записями. Также их можно называть кортежами.

Столбцы в таблице называются полями. Также их можно называть атрибутами.

Поля/атрибуты — это категории, используемые для определения данных в записи (строке).

Реляционная база данных — база данных, содержащая множество таблиц, связанных между собой при помощи ключевых полей (первичных и внешних ключей, уникальных ссылок на запись в другой таблице).

Первичный ключ — это уникальный идентификатор записи в таблице. Он должен быть уникальным и не должен быть пустым.

Внешний ключ - это поле или комбинация полей в таблице, значение которого соответствует первичному ключу в другой таблице. 

1 , ∞ - символы, обозначающие связь между таблицами «Один ко многим». Где 1, как правило, находится первичны ключь. Там где ∞, запись может встречаться множество раз.

Составной ключ — для определения первичного ключа, в некоторых случаях, может использоваться несколько первичных ключей (В последствии, это сочетание рассматривается как единый первичный ключ). Эти ключи могут повторяться в таблице, но их сочетание уникально, и не повторяется более 1 раза.

Реляционные системы управления базами данных РСУБД — это интерфейс, применяемый для упрощения доступа пользователя к SQL базе данных.

SQL – язык структурированных запросов.

Синтаксис запроса — последовательность и выбор операторов SQL для правильной интерпретации браузером SQL.

Запрос — набор команд, который возвращает информацию из базы данных в виде записей.

Оператор SQL – это выполняемый РСУБД любой допустимый фрагмент кода.

Условие — это часть запроса, содержащая по крайней мере одно ключевое слово (например SELECT) и соответствующую информацию (например: название таблицы, столбца, поля, даты, ключа и т.д.), которая будет использоваться вместе с этим ключевым словом.

«Сущность - связь» (ERD – Entity – Relationship сущность-связь) — схема базы данных, изображающая взаимосвязь между таблицами и их первичными и внешними ключами.

Операторы — Это ключевые слова SQL для выполнения арифметических операций, выбора подмножеств полей, для сравнения значений полей. Которые следует использовать вместе с условиями SQL (например таких, как SELECT и WHERE).

Операторы сравнения: = равно; >Больше; <Меньше; >=Больше или равно; <=Меньше или равно; <>Не равно;
Логические операторы: BETWEEN (Между); IN (В); LIKE (Как); AND (И); OR (Или);
Арифметические операторы: +Сложение; -Вычитание; /Деление; *Умножение; %Остаток от деления.

Нормализация — это процесс организации данных в реляционной базе данных. Цель нормализации, упростить базу данных, распределив информацию по нескольким связанным таблицам, дабы избежать многократного повторения записи, за счет чего, уменьшить объем файла базы данных, и скорость обработки информации.

Функции в SQL – это специальные ключевые слова, которые принимают определенные параметры, выполняют определенные операции (например, вычисление или изменение данных в поле), и возвращают результат в виде значения.

Распространенные типы функций:
Строковые функции: INSTR() LENGTH() LOWER() LTRIM() REPLACE() RTRIM() SUBSTR() TRIM() - оператор конкатенации || UPPER()
Функции даты и времени: DATE() DATETIME() JULIANDAY() STRFTIME() TIME() ‘NOW’
Агрегатные функции: AVG() COUNT() MAX() MIN() SUM()

Конкатенация строк — объединение двух или более строк.

Недетерминированные функции (например NOW) — функции, при каждом вводе которых, всегда даются новые результаты.

Стр.140
Агрегатные функции (например: SUM(), AVG(), MIN(), MAX(), COUNT())— воздействуют на значения столбца, чтобы получить единое результирующее значение с помощью различных  математических операций.

Стр.166
Представления — Сохраненная часть кода (как-бы в метод), для последующего многократного использования.

