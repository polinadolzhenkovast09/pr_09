# pr_09
## Геопространственный анализ
## Цель
Научиться создавать временные таблицы, работать с ними, а также строить карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.

##  Задание
Определение ближайшего дилерского центра для каждого клиента. Маркетологи компании пытаются повысить вовлеченность клиентов, необходимо им помочь найти ближайший дилерский центр. Разработчикам продукта также интересно знать, каково среднее расстояние между каждым клиентом и ближайшим к нему дилерским центром.
## Ход работы
1.Проверить наличие геопространственных данных в базе данных.

2.Создать временную таблицу с координатами долготы и широты для каждого клиента.

3.Создать аналогичную таблицу для каждого дилерского центра.

4.Соединить эти таблицы, чтобы рассчитать расстояние от каждого клиента до каждого дилерского центра (в киллометрах).

5.Определить ближайший дилерский центр для каждого клиента.

6.Провести выгрузку полученного результата из временной таблицы в CSV.

7.Построить карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.

8.Удалить временные таблицы
## 1. Создаем временную таблицу с координатами долготы и широты для каждого клиента.
````
create temp table customer_points as(
select customer_id, 
point(longitude, latitude) as lng_lat_point
from customers
where longitude is not null
and latitude is not null);
select * from customer_points
````
![2025-05-05_15-11-17](https://github.com/user-attachments/assets/4ef57a07-591b-4f36-91cf-71dc93cf4b08)

## 2. Создаем аналогичную таблицу для каждого дилерского центра.
````
create temp table dealership_points as(
select dealership_id,
point(longitude, latitude) as lng_lat_point
from dealerships);
select * from dealership_points
````
![2025-05-05_15-12-11](https://github.com/user-attachments/assets/05c0fb7c-6c95-4911-b33b-ffdba3ca6aea)

## 3.Создать аналогичную таблицу для каждого дилерского центра.
````
create temp table customer_dealership_distance as(
select customer_id,
dealership_id,
c.lng_lat_point <@> d.lng_lat_point as distance
from customer_points c
cross join dealership_points d);
select * from customer_dealership_distance
````
![image](https://github.com/user-attachments/assets/c13428fc-f84c-44ad-9a99-3efd1cc8062b) 

## 5.Определить ближайший дилерский центр для каждого клиента.
````
create temp table closest_dealerships as(
select distinct on (customer_id)
customer_id,
dealership_id,
distance
from customer_dealership_distance
order by customer_id, distance);
select * from closest_dealerships
````
![image](https://github.com/user-attachments/assets/d72947ab-4752-455d-ae5c-4d4b7cfdd55a)
## 6.Провести выгрузку полученного результата из временной таблицы в CSV.
![image](https://github.com/user-attachments/assets/04347559-20c3-41d5-a0a6-95ec09701919)

## 7.Построить карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.
Подключаемся к базе данных Postgres. Создаем датасет и добавляем туда таблицы  customers, dealerships и sales. Далее добавляем поля coordinat_cust и coordinat_deal.Создаем чарт (тип чарта карта).Добавляем два слоя coordinat_cust и coordinat_deal и создаем тултипы.

![image](https://github.com/user-attachments/assets/bcfcb15e-d748-49bd-8319-943a4aacd931)

Ссылка на карту:https://datalens.yandex/anjb8ldp3erkw

## Вывод
Научились создавать временные таблицы, выполнили запросы и постороили карту клиентов и сервисных центров в облачной визуализации Yandex DataLence.











