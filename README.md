John Popenuck

James Harris

SELECT Basics
```
SELECT population FROM world
  WHERE name = 'Germany';

SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Iceland', 'Denmark');```

SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```

SELECT from WORLD
```
SELECT name, continent, population FROM world

SELECT name FROM world
WHERE population>200000000

SELECT name, gdp/population FROM world
WHERE population > 200000000

SELECT name, population/1000000 FROM world
WHERE continent = 'South America'

SELECT name, population FROM  world
WHERE name IN ('France', 'Germany', 'Italy')

SELECT name FROM world
WHERE name LIKE '%United%'

SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000

SELECT name, population, area FROM world
WHERE area > 3000000 XOR population > 250000000

SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2) FROM world
WHERE continent = 'South America'

SELECT name, ROUND(gdp/population, -3) FROM world
WHERE gdp > 1000000000000

SELECT name,
       CASE WHEN continent='Oceania' THEN 'Australasia'
            ELSE continent END
  FROM world
 WHERE name LIKE 'N%'

SELECT name,
      CASE WHEN continent='Europe' OR continent='Asia' THEN 'Eurasia'
           WHEN continent='North America' OR continent='South America' OR continent='Caribbean' THEN 'America'
           ELSE continent END
 FROM world
WHERE name LIKE 'A%' OR name LIKE 'B%'

SELECT name, continent,
  CASE WHEN continent='Oceania' THEN 'Australasia'
       WHEN name='Turkey' OR continent='Eurasia' THEN 'Europe/Asia'
       WHEN continent='Caribbean' AND name LIKE 'B%' THEN 'North America'
       WHEN continent='Caribbean' AND name NOT LIKE 'B%' THEN 'South America'
  ELSE continent END
  FROM world
WHERE tld IN ('.ag','.ba','.bb','.ca','.cn','.nz','.ru','.tr','.uk')
ORDER BY name
```
SELECT from NOBEL
```

SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950

 SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'

SELECT yr, subject FROM nobel
   WHERE winner = 'Albert Einstein'

SELECT winner FROM nobel
   WHERE subject = "Peace"
   AND yr >= 2000

SELECT * FROM nobel
   WHERE subject = "Literature"
   AND yr BETWEEN 1980 AND 1989

SELECT * FROM nobel
    WHERE winner IN ('Theodore Roosevelt',
                     'Woodrow Wilson',
                     'Jimmy Carter')

SELECT winner FROM nobel
    WHERE winner LIKE 'John%'

SELECT * FROM nobel
    WHERE (subject = 'Physics' AND yr = 1980)
    OR (subject='Chemistry' AND yr=1984)

SELECT * FROM nobel
    WHERE yr=1980
    AND subject NOT IN ('Chemistry', 'Medicine')

SELECT * FROM nobel
    WHERE (subject='Medicine' AND yr<1910)
    OR (subject='Literature' AND yr>=2004)

SELECT * FROM nobel
    WHERE winner='Peter Grünberg'

SELECT * FROM nobel
    WHERE winner='Eugene O\'Neill'

SELECT winner, yr, subject FROM nobel
    WHERE winner LIKE 'Sir%'
    ORDER BY yr DESC, winner

SELECT winner, subject
     FROM nobel
     WHERE yr=1984
     ORDER BY subject IN ('Physics','Chemistry'),subject,winner


```
JOIN and UEFA EURO 2012
```
SELECT matchid, player FROM goal
  WHERE teamid = 'GER'

SELECT id,stadium,team1,team2
  FROM game
  WHERE id=1012

SELECT player,teamid,stadium,mdate
  FROM game JOIN goal ON (id=matchid)
  WHERE teamid='GER'

SELECT team1, team2, player
  FROM game JOIN goal ON (id=matchid)
  WHERE player LIKE 'Mario%'

SELECT player, teamid, coach, gtime
  FROM goal JOIN eteam ON teamid=id
 WHERE gtime<=10

SELECT mdate, teamname
  FROM game JOIN eteam ON (team1=eteam.id)
  WHERE coach='Fernando Santos'

SELECT player
  FROM game JOIN goal ON (id=matchid)
  WHERE stadium='National Stadium, Warsaw'

SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id
    WHERE (team1='GER' OR team2='GER') AND teamid!='GER'

SELECT teamname, COUNT(player)
  FROM eteam JOIN goal ON id=teamid
 GROUP BY teamname

SELECT stadium, COUNT(matchid)
FROM game JOIN goal ON id=matchid
GROUP BY stadium

SELECT matchid,mdate, count(matchid)
  FROM game JOIN goal ON matchid = id
 WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid, mdate

SELECT id, mdate, COUNT(matchid)
FROM game JOIN goal ON id=matchid
WHERE teamid='GER'
GROUP BY id, mdate

SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS score2

  FROM game LEFT JOIN goal ON matchid = id
  GROUP BY mdate, team1, team2
  ORDER BY mdate, matchid, team1, team2
```
[Movie JOIN Operations](http://sqlzoo.net/wiki/More_JOIN_operations)
```
1.
SELECT id, title
  FROM movie
  WHERE yr=1962

2.
SELECT yr FROM movie
  WHERE title='Citizen Kane'

3.
SELECT id, title, yr FROM movie
  WHERE title LIKE '%Star Trek%'
  ORDER BY yr

4.
SELECT title FROM movie
  WHERE id IN ('11768', '11955', '21191')

5.
SELECT id FROM actor
  WHERE name='Glenn Close'

6.
SELECT id FROM movie
  WHERE title='Casablanca'

7.
SELECT name
  FROM movie JOIN casting ON movie.id=movieid
             JOIN actor ON actor.id=actorid
  WHERE movieid=11768

8.
SELECT name
  FROM movie JOIN casting ON movie.id=movieid
             JOIN actor ON actor.id=actorid
  WHERE title='Alien'

9.
SELECT title
  FROM movie JOIN casting ON movie.id=movieid
           JOIN actor ON actor.id=actorid
  WHERE name='Harrison Ford'

10.
SELECT title
  FROM movie JOIN casting ON movie.id=movieid
           JOIN actor ON actor.id=actorid
  WHERE name='Harrison Ford' AND ord!=1

11.
SELECT title, name
  FROM movie JOIN casting ON movie.id=movieid
             JOIN actor ON actor.id=actorid
  WHERE yr=1962 AND ord=1

12.
SELECT title, name
FROM movie JOIN casting ON movie.id=movieid
JOIN actor ON actor.id=actorid
WHERE actor.name='Julie Andrews' IN (
  SELECT id FROM actor.name
  WHERE name='Julie Andrews')

  13.

  Long solution:
  ```sql
  SELECT andrews_movies.title, leading_actors.name FROM

  (SELECT DISTINCT movie.title, movieid
  FROM actor JOIN casting ON actorid=actor.id JOIN movie ON movie.id = movieid
  WHERE actor.name='Julie Andrews') AS andrews_movies

  JOIN

  (SELECT movieid, actor.name
  FROM actor JOIN casting ON actorid=actor.id JOIN movie ON movie.id = movieid
  WHERE ord=1) AS leading_actors

  ON andrews_movies.movieid=leading_actors.movieid
  ```
  Short solution:
  ```sql
  SELECT title, name
    FROM actor JOIN casting ON id = actorid JOIN movie ON movie.id = movieid
    WHERE ord=1 AND movieid IN (SELECT movieid
    FROM actor JOIN casting ON id = actorid JOIN movie ON movie.id = movieid
    WHERE name = 'Julie Andrews')
  ```

  14.
  ```sql
  SELECT name
  FROM actor JOIN casting ON id = actorid JOIN movie ON movie.id = movieid
  WHERE ord=1
  GROUP BY name
  HAVING COUNT(name) >= 30
  ```

  15.
  ```
  SELECT title, COUNT(actorid)
  FROM actor JOIN casting ON id = actorid JOIN movie ON movie.id = movieid
  WHERE yr=1978
  GROUP BY title
  ORDER BY COUNT(actorid) DESC, title
  ```
  16.
  ```
  SELECT name
  FROM (SELECT movieid
    FROM actor JOIN casting ON id = actorid
    WHERE name='Art Garfunkel') AS art_movies

  JOIN casting ON art_movies.movieid=casting.movieid
  JOIN actor ON id=actorid WHERE name != 'Art Garfunkel'
  ```
[SUM and COUNT](http://sqlzoo.net/wiki/SUM_and_COUNT)

1.
```sql
SELECT SUM(population)
FROM world
```

2.
```sql
SELECT DISTINCT continent
FROM world
```

3.
```sql
SELECT SUM(gdp)
FROM world
WHERE continent='Africa'
```

4.
```sql
SELECT COUNT(name)
FROM world
WHERE area >= 1000000
```

5.
```sql
SELECT SUM(population)
FROM world
WHERE name IN ('France','Germany','Spain')
```

6.
```sql
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
```

7.
```sql
SELECT continent, COUNT(name)
FROM world
WHERE population > 10000000
GROUP BY continent
```

8.
```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) > 100000000
```

[SELECT within SELECT](http://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)

1.
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

2.
```sql
SELECT name FROM world
WHERE continent='Europe' AND gdp/population >
(SELECT gdp/population FROM world
WHERE name='United Kingdom')
```

3.
```sql
SELECT name, continent FROM world
WHERE continent IN
(SELECT continent FROM world
WHERE name IN ('Argentina','Australia'))
ORDER BY name
```

4.
```sql
SELECT name, population FROM world
WHERE population >
(SELECT population FROM world WHERE name='Canada')
AND population < (SELECT population FROM world WHERE name='Poland')
```

5.
```sql
SELECT name,
CONCAT(
ROUND(
population/(SELECT population FROM world WHERE name='Germany')*100,
0),
'%')
FROM world
WHERE continent = 'Europe'
```

6.
```sql
SELECT name
  FROM world
 WHERE population >= ALL(SELECT population
                           FROM world
                          WHERE population>0)
```

7.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```

8.
```sql
SELECT continent, name FROM world x
WHERE name <=ALL
(SELECT name FROM world y
WHERE y.continent=x.continent)
```

9.
```sql
SELECT name,continent, population FROM world x
WHERE 25000000 >= ALL
(SELECT population FROM world y
WHERE y.continent=x.continent)
```

10.
```sql
SELECT name,continent FROM world x
WHERE population > ALL
(SELECT population*3 FROM world y
WHERE x.continent=y.continent AND x.name != y.name)
```

[Using Null](http://sqlzoo.net/wiki/Using_Null)

1.
```sql
SELECT name FROM teacher
WHERE dept IS NULL
```

2.
```sql
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```

3.
```sql
SELECT teacher.name, dept.name
 FROM teacher LEFT OUTER JOIN dept
  ON (teacher.dept=dept.id)
```
4.
```sql
SELECT teacher.name, dept.name
 FROM teacher RIGHT OUTER JOIN dept
  ON (teacher.dept=dept.id)
```
5.
```sql
SELECT name, COALESCE(mobile, '07986 444 2266') FROM teacher
```
6.
```sql
SELECT teacher.name, COALESCE(dept.name, 'None')
 FROM teacher LEFT JOIN dept
  ON (teacher.dept=dept.id)
```
7.
```sql
SELECT COUNT(name), COUNT(mobile) FROM teacher
```
8.
```sql
SELECT dept.name, COUNT(teacher.name)
FROM teacher RIGHT JOIN dept ON teacher.dept=dept.id
GROUP BY dept.name
```
9.
```sql
SELECT teacher.name, CASE
       WHEN dept.id=1 THEN 'Sci'
       WHEN dept.id=2 THEN 'Sci'
       ELSE 'Art'
       END
FROM teacher LEFT JOIN dept ON teacher.dept=dept.id
```
10.
```sql
SELECT teacher.name, CASE
       WHEN dept.id=1 THEN 'Sci'
       WHEN dept.id=2 THEN 'Sci'
       WHEN dept.id=3 THEN 'Art'
       ELSE 'None'
       END
FROM teacher LEFT JOIN dept ON teacher.dept=dept.id
```
