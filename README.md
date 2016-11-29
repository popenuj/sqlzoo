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
