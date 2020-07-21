# Задание на вакансию Full-stack web-разработчик PHP

> Задача №.1

> Решение без оптимизации структуры таблиц:
```
SELECT t1.name AS 'Имя',
COUNT (t2.id) AS 'Число номеров'
FROM `users` t1 
LEFT JOIN `phone_numbers` t2 ON t2.user_id = t1.id 
WHERE t1.gender = 2
AND TIMESTAMPDIFF(YEAR, FROM_UNIXTIME(t1.birth_date), NOW()) BETWEEN 18 AND 22 
GROUP BY t1.id;
```

> Решение с оптимизацией структуры таблиц и запроса:
```
CREATE TABLE `users` (
    `id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(100) DEFAULT NULL,
    `gender` TINYINT(2) UNSIGNED NOT NULL COMMENT '0 - не указан, 1 - мужчина, 2 -
    женщина.',
    `birth_date` DATE NOT NULL,
    PRIMARY KEY (`id`),
    KEY `i1` (`gender`,`birth_date`)
);
CREATE TABLE `phone_numbers` (
	`id` INT(11) UNSIGNED NOT NULL AUTO_INCREMENT,
	`user_id` INT(11) UNSIGNED NOT NULL,
	`phone` VARCHAR(20) UNIQUE DEFAULT NULL COMMENT 'Уникальное значение.',
	PRIMARY KEY (`id`),
	KEY `i1` (`user_id`)
);

SELECT t1.name AS 'Имя',
COUNT (t2.id) AS 'Число номеров'
FROM `users` t1 
LEFT JOIN `phone_numbers` t2 ON t2.user_id = t1.id 
WHERE t1.gender = 2
AND TIMESTAMPDIFF(YEAR, t1.birth_date, NOW()) BETWEEN 18 AND 22
GROUP BY t1.id;
```

> Задача №.2

```$php
/**
 * @param string $url
 *
 * @return string
 *
 * @author Андрей Чепурнов
 */
function urlparsing($url)
{
	$parsedurl = parse_url($url);
	parse_str($parsedurl['query'], $params);
	$params = array_diff($params, [3]);
	asort($params);
	$params['url'] = $parsedurl['path'];
	$paramsstr = htmlspecialchars(http_build_query($params));

	return $parsedurl['scheme'].'://'.$parsedurl['host'].'/'.$paramsstr;
}
```

> Задача №.3

```$php
/**
 * Конфигурация БД
 */
CONST DB_HOST     = "localhost";
CONST DB_USER     = "root";
CONST DB_PASSWORD = "123123";
CONST DB_NAME     = "database";

/**
 * @param string $user_ids
 *
 * @return array $data
 *
 * @author автор
 * @review Андрей Чепурнов
 */
function load_users_data($user_ids) {
	$data = [];
	$db = mysqli_connect(DB_HOST, DB_USER, DB_PASSWORD, DB_NAME);
	$sql = mysqli_query($db, "SELECT * FROM users WHERE id IN ($user_ids);");
	while($obj = $sql->fetch_object()){
		$data[$obj->id] = $obj->name;
	}
	mysqli_close($db);
	
	return $data;
}

/**
 * Как правило, в $_GET['user_ids'] должна приходить строка
 * с номерами пользователей через запятую, например: 1,2,17,48
 */
$htmlstr = "";
$data = load_users_data($_GET['user_ids']);
foreach ($data as $user_id=>$name) {
	$htmlstr .= "<a href=\"/show_user.php?id=$user_id\">$name</a><br/>";
}
echo $htmlstr;
```

> Вопрос №2

- Примеры используемой литературы:
    - PHP 5 Алексей Костарев, Дмитрий Котеров  Санкт Петербург 2014
    - PHP Ajax Cookbook R. Rajesh Jeba Anbiah, Roshan Bhattarai, Milan Sedliak 2011
    - O'Reilly jQuery Andreas Vdovkin 2011
    - O'Reilly CSS Eric A. Meyer 2011
    - Yii2 by Example Fabrizio Caldarelli 2015
    - Modx Web development Antano Solar John 2009
    - C als erste Programmiersprache Joachim Goll, ulrich Broeckl, Manfred Dausmann 2007
    
- Примеры используемых веб-ресурсов:
    - https://stackoverflow.com/
    - https://habr.com/
    - https://www.php.net/
    - https://www.w3schools.com/
    - https://www.yiiframework.com/