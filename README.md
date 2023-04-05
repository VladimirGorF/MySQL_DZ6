1.	Создайте функцию, которая принимает кол-во сек и форматирует их в кол-во дней, часов, минут и секунд.
Пример: 123456 ->'1 days 10 hours 17 minutes 36 seconds '
<img width="2048" alt="Снимок экрана 2023-04-04 в 17 50 09" src="https://user-images.githubusercontent.com/110591063/229831462-7ce9af2d-2731-41ec-920e-d6ae05ebd094.png">





2.	Выведите только четные числа от 1 до 10 включительно. (Через функцию / процедуру)
Пример: 2,4,6,8,10 (можно сделать через шаг +  2: х = 2, х+=2)

<img width="2048" alt="Снимок экрана 2023-04-02 в 15 27 58" src="https://user-images.githubusercontent.com/110591063/229352790-6ac95f15-8abd-4d74-826e-e56f5d7c3a1c.png">



Создать процедуру, которая решает следующую задачу
Выбрать для одного пользователя 5 пользователей в случайной комбинации, которые удовлетворяют хотя бы одному критерию:
а) из одного города
б) состоят в одной группе
в) друзья друзей

DROP PROCEDURE IF EXISTS tor;
DELIMITER //
CREATE PROCEDURE tor(IN num INT) -- здесь num подразумевает id персонажа
BEGIN

--   выборка по городам
SELECT user_id
FROM profiles
WHERE hometown =(SELECT hometown FROM profiles WHERE user_id = num) AND user_id != num

UNION

-- выборка по сообществам
SELECT DISTINCT user_id
FROM users_communities
WHERE community_id IN (SELECT community_id FROM users_communities WHERE user_id = num) AND user_id != num

-- выборка по друзьям друзей
UNION
 (SELECT initiator_user_id
 FROM friend_requests

 WHERE (target_user_id IN ( SELECT target_user_id
 FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved")

 OR initiator_user_id IN (SELECT initiator_user_id
 FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved")

OR initiator_user_id IN (SELECT target_user_id
FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved")

OR target_user_id IN ( SELECT initiator_user_id
FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved"))

AND status = "approved" AND initiator_user_id != num

UNION

 SELECT  target_user_id
 FROM friend_requests

 WHERE (target_user_id IN ( SELECT target_user_id
 FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved")

 OR initiator_user_id IN (SELECT initiator_user_id
 FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved")

OR initiator_user_id IN (SELECT target_user_id
FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved")

OR target_user_id IN ( SELECT initiator_user_id
FROM friend_requests WHERE (target_user_id = num OR initiator_user_id = num) AND status = "approved"))

AND status = "approved" AND target_user_id != num )

ORDER BY RAND();
END//

CALL tor(1);








Создать функцию, вычисляющей коэффициент популярности пользователя
Создать процедуру для добавления нового пользователя с профилем

