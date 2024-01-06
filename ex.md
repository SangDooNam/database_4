(base) sangdoonam@SangDoos-iMac table-relationships-pytech % psql -U postgres
psql (14.10 (Homebrew))
Type "help" for help.

postgres=# CREATE DATABASE map;
CREATE DATABASE
postgres=# \c map
You are now connected to database "map" as user "postgres".
map=# CREATE TABLE country(id INT SERIAL PRIMARY KEY, )
map-# CREATE TABLE country(id INT SERIAL PRIMARY KEY, name VARCHAR(50), population INTEGER, last_status_change DATE);
ERROR:  syntax error at or near "SERIAL"
LINE 1: CREATE TABLE country(id INT SERIAL PRIMARY KEY, )
                                    ^
map=# CREATE TABLE country(id INT SERIAL PRIMARY KEY, name VARCHAR(50), population INTEGER, last_status_change DATE);
ERROR:  syntax error at or near "SERIAL"
LINE 1: CREATE TABLE country(id INT SERIAL PRIMARY KEY, name VARCHAR...
                                    ^
map=# CREATE TABLE country(id SERIAL PRIMARY KEY, name VARCHAR(50), population INTEGER, last_status_change DATE);
CREATE TABLE
map=# \d country
                                         Table "public.country"
       Column       |         Type          | Collation | Nullable |               Default               
--------------------+-----------------------+-----------+----------+-------------------------------------
 id                 | integer               |           | not null | nextval('country_id_seq'::regclass)
 name               | character varying(50) |           |          | 
 population         | integer               |           |          | 
 last_status_change | date                  |           |          | 
Indexes:
    "country_pkey" PRIMARY KEY, btree (id)

map=# INSERT INTO country (id, name, population, last_status_change) VALUES ()
map-# ;
ERROR:  syntax error at or near ")"
LINE 1: ...country (id, name, population, last_status_change) VALUES ()
                                                                      ^
map=# INSERT INTO country (id, name, population, last_status_change) VALUES
map-# (Germany, 83190556, 1990-10-03),
map-# (France, 6741300, 1958-10-04),
map-# (Namibia, 2550226, 1990-03-21),
map-# (Uruguay, 3518552, 1830-07-18),
map-# (Kazakhstan, 18711560, 1995-08-30);
ERROR:  column "germany" does not exist
LINE 2: (Germany, 83190556, 1990-10-03),
         ^
map=# \d country
                                         Table "public.country"
       Column       |         Type          | Collation | Nullable |               Default               
--------------------+-----------------------+-----------+----------+-------------------------------------
 id                 | integer               |           | not null | nextval('country_id_seq'::regclass)
 name               | character varying(50) |           |          | 
 population         | integer               |           |          | 
 last_status_change | date                  |           |          | 
Indexes:
    "country_pkey" PRIMARY KEY, btree (id)

map=# INSERT INTO country (name, population, last_status_change) VALUES
map-# (Germany, 83190556, 1990-10-03),(France, 6741300, 1958-10-04),(Namibia, 2550226, 1990-03-21),(Uruguay, 3518552, 1830-07-18),(Kazakhstan, 18711560, 1995-08-30);
ERROR:  column "germany" does not exist
LINE 2: (Germany, 83190556, 1990-10-03),(France, 6741300, 1958-10-04...
         ^
map=# INSERT INTO country (id, name, population, last_status_change) VALUES   
('Germany', '83190556', '1990-10-03'),
('France', '6741300', '1958-10-04'),
('Namibia', '2550226', '1990-03-21'),
('Uruguay', '3518552', '1830-07-18'),
('Kazakhstan', '18711560', '1995-08-30');
ERROR:  INSERT has more target columns than expressions
LINE 1: INSERT INTO country (id, name, population, last_status_chang...
                                                   ^
map=# INSERT INTO country (name, population, last_status_change) VALUES
('Germany', '83190556', '1990-10-03'),
('France', '6741300', '1958-10-04'),
('Namibia', '2550226', '1990-03-21'),
('Uruguay', '3518552', '1830-07-18'),
('Kazakhstan', '18711560', '1995-08-30');
INSERT 0 5
map=# \d country
                                         Table "public.country"
       Column       |         Type          | Collation | Nullable |               Default               
--------------------+-----------------------+-----------+----------+-------------------------------------
 id                 | integer               |           | not null | nextval('country_id_seq'::regclass)
 name               | character varying(50) |           |          | 
 population         | integer               |           |          | 
 last_status_change | date                  |           |          | 
Indexes:
    "country_pkey" PRIMARY KEY, btree (id)

map=# SELECT * FROM country;
 id |    name    | population | last_status_change 
----+------------+------------+--------------------
  1 | Germany    |   83190556 | 1990-10-03
  2 | France     |    6741300 | 1958-10-04
  3 | Namibia    |    2550226 | 1990-03-21
  4 | Uruguay    |    3518552 | 1830-07-18
  5 | Kazakhstan |   18711560 | 1995-08-30
(5 rows)

map=# UPDATE country SET population = 67413000 WHERE id = 2;
UPDATE 1
map=# SELECT * FROM country;
 id |    name    | population | last_status_change 
----+------------+------------+--------------------
  1 | Germany    |   83190556 | 1990-10-03
  3 | Namibia    |    2550226 | 1990-03-21
  4 | Uruguay    |    3518552 | 1830-07-18
  5 | Kazakhstan |   18711560 | 1995-08-30
  2 | France     |   67413000 | 1958-10-04
(5 rows)

map=# CREATE TABLE city(id SERIAL PRIMARY KEY, name VARCHAR(20), area DECIMAL, is_capital BOOLEAN, country_id INTEGER REFERENCE country(id));
ERROR:  syntax error at or near "REFERENCE"
LINE 1: ...a DECIMAL, is_capital BOOLEAN, country_id INTEGER REFERENCE ...
map=# CREATE TABLE city(id SERIAL PRIMARY KEY, name VARCHAR(20), area DECIMAL, is_capital BOOLEAN, country_id INTEGER REFERENCES country(id));
CREATE TABLE
map=# INSERT INTO city(name, area, is_capita, country_id) VALUES
map-# ('Nur-Sultan', '810.2','t','5'),
map-# ('Montevideo', '201', 't', '4'),
map-# ('Florida', '8.2', 'f', '4'),
map-# ('Windhoek', '5133', 't', '3'),
map-# ('Swakopmund','196.3','f','3'),
map-# ('Marseille', '240.62', 'f' '2'),
map-# ('Berlin', '891.7', 't', '1');
ERROR:  syntax error at or near "'2'"
LINE 7: ('Marseille', '240.62', 'f' '2'),
                                    ^
map=# INSERT INTO city(name, area, is_capita, country_id) VALUES
('Nur-Sultan', '810.2','t','5'),
('Montevideo', '201', 't', '4'),
('Florida', '8.2', 'f', '4'),
('Windhoek', '5133', 't', '3'),
('Swakopmund','196.3','f','3'),
('Marseille', '240.62', 'f', '2'),
('Berlin', '891.7', 't', '1');
ERROR:  column "is_capita" of relation "city" does not exist
LINE 1: INSERT INTO city(name, area, is_capita, country_id) VALUES
                                     ^
map=# INSERT INTO city(name, area, is_capital, country_id) VALUES
('Nur-Sultan', '810.2','t','5'),
('Montevideo', '201', 't', '4'),
('Florida', '8.2', 'f', '4'),
('Windhoek', '5133', 't', '3'),
('Swakopmund','196.3','f','3'),
('Marseille', '240.62', 'f', '2'),
('Berlin', '891.7', 't', '1');
INSERT 0 7
map=# SELECT * FROM city
map-# ;
 id |    name    |  area  | is_capital | country_id 
----+------------+--------+------------+------------
  1 | Nur-Sultan |  810.2 | t          |          5
  2 | Montevideo |    201 | t          |          4
  3 | Florida    |    8.2 | f          |          4
  4 | Windhoek   |   5133 | t          |          3
  5 | Swakopmund |  196.3 | f          |          3
  6 | Marseille  | 240.62 | f          |          2
  7 | Berlin     |  891.7 | t          |          1
(7 rows)

map=# SELECT *
map-# FROM country
map-# LEFT JOIN city;
ERROR:  syntax error at or near ";"
LINE 3: LEFT JOIN city;
                      ^
map=# SELECT *
FROM country
LEFT JOIN city
map-# ON country.common_table = city.common_column;
ERROR:  column country.common_table does not exist
LINE 4: ON country.common_table = city.common_column;
           ^
map=# SELECT *
FROM country
LEFT JOIN city;
ERROR:  syntax error at or near ";"
LINE 3: LEFT JOIN city;
                      ^
map=# SELECT *
FROM country
LEFT JOIN city
map-# ON country_id = id;
ERROR:  column reference "id" is ambiguous
LINE 4: ON country_id = id;
                        ^
map=# SELECT *
FROM country
LEFT JOIN city
ON country.id = city.country_id;
 id |    name    | population | last_status_change | id |    name    |  area  | is_capital | country_id 
----+------------+------------+--------------------+----+------------+--------+------------+------------
  5 | Kazakhstan |   18711560 | 1995-08-30         |  1 | Nur-Sultan |  810.2 | t          |          5
  4 | Uruguay    |    3518552 | 1830-07-18         |  2 | Montevideo |    201 | t          |          4
  4 | Uruguay    |    3518552 | 1830-07-18         |  3 | Florida    |    8.2 | f          |          4
  3 | Namibia    |    2550226 | 1990-03-21         |  4 | Windhoek   |   5133 | t          |          3
  3 | Namibia    |    2550226 | 1990-03-21         |  5 | Swakopmund |  196.3 | f          |          3
  2 | France     |   67413000 | 1958-10-04         |  6 | Marseille  | 240.62 | f          |          2
  1 | Germany    |   83190556 | 1990-10-03         |  7 | Berlin     |  891.7 | t          |          1
(7 rows)

map=# \d
               List of relations
 Schema |      Name      |   Type   |  Owner   
--------+----------------+----------+----------
 public | city           | table    | postgres
 public | city_id_seq    | sequence | postgres
 public | country        | table    | postgres
 public | country_id_seq | sequence | postgres
(4 rows)

map=# SELECT *
map-# FROM city
map-# LEFT JOIN country
map-# ON city.country_id = country.id;
 id |    name    |  area  | is_capital | country_id | id |    name    | population | last_status_change 
----+------------+--------+------------+------------+----+------------+------------+--------------------
  1 | Nur-Sultan |  810.2 | t          |          5 |  5 | Kazakhstan |   18711560 | 1995-08-30
  2 | Montevideo |    201 | t          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  3 | Florida    |    8.2 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  4 | Windhoek   |   5133 | t          |          3 |  3 | Namibia    |    2550226 | 1990-03-21
  5 | Swakopmund |  196.3 | f          |          3 |  3 | Namibia    |    2550226 | 1990-03-21
  6 | Marseille  | 240.62 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
  7 | Berlin     |  891.7 | t          |          1 |  1 | Germany    |   83190556 | 1990-10-03
(7 rows)

map=# \i data.sql
TRUNCATE TABLE
psql:data.sql:3: NOTICE:  truncate cascades to table "city"
TRUNCATE TABLE
ALTER SEQUENCE
ALTER SEQUENCE
INSERT 0 5
INSERT 0 18
map=# SELECT * FROM city;
 id |    name    |  area  | is_capital | country_id 
----+------------+--------+------------+------------
  1 | Nur-Sultan |  810.2 | t          |          5
  2 | Montevideo |    201 | t          |          4
  3 | Florida    |    8.2 | f          |          4
  4 | Windhoek   |   5133 | t          |          3
  5 | Swakopmund |  196.3 | f          |          3
  6 | Marseille  | 240.62 | f          |          2
  7 | Berlin     |  891.7 | t          |          1
  8 | Salto      |   14.2 | f          |          4
  9 | Rio Negro  |    9.3 | f          |          4
 10 | Maldonado  |    4.8 | f          |          4
 11 | Flores     |    5.1 | f          |          4
 12 | Paris      |  105.4 | t          |          2
 13 | Lyon       |  47.87 | f          |          2
 14 | Toulouse   |  118.3 | f          |          2
 15 | Nice       |  71.92 | f          |          2
 16 | Hamburg    | 755.22 | f          |          1
 17 | Munich     | 310.71 | f          |          1
 18 | Frankfurt  | 248.31 | f          |          1
(18 rows)

map=# SELECT * FROM country;
 id |    name    | population | last_status_change 
----+------------+------------+--------------------
  1 | Germany    |   83190556 | 1990-10-03
  2 | France     |   67413000 | 1958-10-04
  3 | Namibia    |    2550226 | 1990-03-21
  4 | Uruguay    |    3518552 | 1830-07-18
  5 | Kazakhstan |   18711560 | 1995-08-30
(5 rows)

map=# SELECT * FROM city WHERE country_id = 1;
 id |   name    |  area  | is_capital | country_id 
----+-----------+--------+------------+------------
  7 | Berlin    |  891.7 | t          |          1
 16 | Hamburg   | 755.22 | f          |          1
 17 | Munich    | 310.71 | f          |          1
 18 | Frankfurt | 248.31 | f          |          1
(4 rows)

map=# SELECT * FROM city WHERE area = MINIMUM;
ERROR:  column "minimum" does not exist
LINE 1: SELECT * FROM city WHERE area = MINIMUM;
                                        ^
map=# SELECT * FROM city WHERE area = (SELECT MIN(area) FROM city);
 id |   name    | area | is_capital | country_id 
----+-----------+------+------------+------------
 10 | Maldonado |  4.8 | f          |          4
(1 row)

map=# SELECT * FROM city LEFT JOIN country ON city.country_id = country.id;
 id |    name    |  area  | is_capital | country_id | id |    name    | population | last_status_change 
----+------------+--------+------------+------------+----+------------+------------+--------------------
  1 | Nur-Sultan |  810.2 | t          |          5 |  5 | Kazakhstan |   18711560 | 1995-08-30
  2 | Montevideo |    201 | t          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  3 | Florida    |    8.2 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  4 | Windhoek   |   5133 | t          |          3 |  3 | Namibia    |    2550226 | 1990-03-21
  5 | Swakopmund |  196.3 | f          |          3 |  3 | Namibia    |    2550226 | 1990-03-21
  6 | Marseille  | 240.62 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
  7 | Berlin     |  891.7 | t          |          1 |  1 | Germany    |   83190556 | 1990-10-03
  8 | Salto      |   14.2 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  9 | Rio Negro  |    9.3 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
 10 | Maldonado  |    4.8 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
 11 | Flores     |    5.1 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
 12 | Paris      |  105.4 | t          |          2 |  2 | France     |   67413000 | 1958-10-04
 13 | Lyon       |  47.87 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
 14 | Toulouse   |  118.3 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
 15 | Nice       |  71.92 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
 16 | Hamburg    | 755.22 | f          |          1 |  1 | Germany    |   83190556 | 1990-10-03
 17 | Munich     | 310.71 | f          |          1 |  1 | Germany    |   83190556 | 1990-10-03
 18 | Frankfurt  | 248.31 | f          |          1 |  1 | Germany    |   83190556 | 1990-10-03
(18 rows)

map=# SELECT * FROM city WHERE area = (SELECT MIN(area) FROM city WHERE is_capital = t);
ERROR:  column "t" does not exist
LINE 1: ...RE area = (SELECT MIN(area) FROM city WHERE is_capital = t);
                                                                    ^
map=# SELECT * FROM city WHERE area = (SELECT MIN(area) FROM city WHERE is_capital = TRUE);
 id | name  | area  | is_capital | country_id 
----+-------+-------+------------+------------
 12 | Paris | 105.4 | t          |          2
(1 row)

map=# SELECT * FROM city;
 id |    name    |  area  | is_capital | country_id 
----+------------+--------+------------+------------
  1 | Nur-Sultan |  810.2 | t          |          5
  2 | Montevideo |    201 | t          |          4
  3 | Florida    |    8.2 | f          |          4
  4 | Windhoek   |   5133 | t          |          3
  5 | Swakopmund |  196.3 | f          |          3
  6 | Marseille  | 240.62 | f          |          2
  7 | Berlin     |  891.7 | t          |          1
  8 | Salto      |   14.2 | f          |          4
  9 | Rio Negro  |    9.3 | f          |          4
 10 | Maldonado  |    4.8 | f          |          4
 11 | Flores     |    5.1 | f          |          4
 12 | Paris      |  105.4 | t          |          2
 13 | Lyon       |  47.87 | f          |          2
 14 | Toulouse   |  118.3 | f          |          2
 15 | Nice       |  71.92 | f          |          2
 16 | Hamburg    | 755.22 | f          |          1
 17 | Munich     | 310.71 | f          |          1
 18 | Frankfurt  | 248.31 | f          |          1
(18 rows)

map=# SELECT * FROM city WHERE area = (SELECT MIN(area) FROM city) WHERE is_capital = TRUE;
ERROR:  syntax error at or near "WHERE"
LINE 1: ...OM city WHERE area = (SELECT MIN(area) FROM city) WHERE is_c...
                                                             ^
map=# SELECT * FROM city WHERE area = (SELECT MIN(area) FROM city WHERE is_capital = TRUE);
 id | name  | area  | is_capital | country_id 
----+-------+-------+------------+------------
 12 | Paris | 105.4 | t          |          2
(1 row)

map=# SELECT country.name city.name area FROM city WHERE area = (SELECT MIN(area) FROM city);
ERROR:  syntax error at or near "."
LINE 1: SELECT country.name city.name area FROM city WHERE area = (S...
                                ^
map=# SELECT country.name, city.name, city.area FROM city
map-# JOIN country ON city.country_id = country.id
map-# WHERE area = (SELECT MIN(area) FROM city);
  name   |   name    | area 
---------+-----------+------
 Uruguay | Maldonado |  4.8
(1 row)

map=# SELECT country.name, city,name, city.area FROM city JOIN country ON city.country_id = country.id
map-# WHERE area = (SELECT MIN(area) FROM city WHERE is_capital = TRUE);
ERROR:  column reference "name" is ambiguous
LINE 1: SELECT country.name, city,name, city.area FROM city JOIN cou...
                                  ^
map=# SELECT country.name, city.name, city.area FROM city JOIN country ON city.country_id = country.id
WHERE area = (SELECT MIN(area) FROM city WHERE is_capital = TRUE);
  name  | name  | area  
--------+-------+-------
 France | Paris | 105.4
(1 row)

map=# SELECT city.name, country.name, country.population FROM country
map-# JOIN city ON country.id = city.country_id WHERE population = (SELECT MAX(population FROM country);
map(# ;
map(# 
map(# SELECT * FROM country;
map(# ;
map(# =
map(# \q
(base) sangdoonam@SangDoos-iMac table-relationships-pytech % psql -U postgres map
psql (14.10 (Homebrew))
Type "help" for help.

map=# SELECT * FROM country JOIN city ON country.id = city.country_id
map-# ORDER BY population DESC LIMIT 3;
 id |  name   | population | last_status_change | id |  name   |  area  | is_capital | country_id 
----+---------+------------+--------------------+----+---------+--------+------------+------------
  1 | Germany |   83190556 | 1990-10-03         |  7 | Berlin  |  891.7 | t          |          1
  1 | Germany |   83190556 | 1990-10-03         | 16 | Hamburg | 755.22 | f          |          1
  1 | Germany |   83190556 | 1990-10-03         | 17 | Munich  | 310.71 | f          |          1
(3 rows)

map=# SELECT city.name, country.name, country.population FROM city JOIN country 
map-# ON city.country_id = country.id ORDER BY population DESC LIMIT 3;
  name   |  name   | population 
---------+---------+------------
 Berlin  | Germany |   83190556
 Hamburg | Germany |   83190556
 Munich  | Germany |   83190556
(3 rows)

map=# SELECT * FROM city LEFT JOIN country ON city.country_id = country.id;
 id |    name    |  area  | is_capital | country_id | id |    name    | population | last_status_change 
----+------------+--------+------------+------------+----+------------+------------+--------------------
  1 | Nur-Sultan |  810.2 | t          |          5 |  5 | Kazakhstan |   18711560 | 1995-08-30
  2 | Montevideo |    201 | t          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  3 | Florida    |    8.2 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  4 | Windhoek   |   5133 | t          |          3 |  3 | Namibia    |    2550226 | 1990-03-21
  5 | Swakopmund |  196.3 | f          |          3 |  3 | Namibia    |    2550226 | 1990-03-21
  6 | Marseille  | 240.62 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
  7 | Berlin     |  891.7 | t          |          1 |  1 | Germany    |   83190556 | 1990-10-03
  8 | Salto      |   14.2 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
  9 | Rio Negro  |    9.3 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
 10 | Maldonado  |    4.8 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
 11 | Flores     |    5.1 | f          |          4 |  4 | Uruguay    |    3518552 | 1830-07-18
 12 | Paris      |  105.4 | t          |          2 |  2 | France     |   67413000 | 1958-10-04
 13 | Lyon       |  47.87 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
 14 | Toulouse   |  118.3 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
 15 | Nice       |  71.92 | f          |          2 |  2 | France     |   67413000 | 1958-10-04
 16 | Hamburg    | 755.22 | f          |          1 |  1 | Germany    |   83190556 | 1990-10-03
 17 | Munich     | 310.71 | f          |          1 |  1 | Germany    |   83190556 | 1990-10-03
 18 | Frankfurt  | 248.31 | f          |          1 |  1 | Germany    |   83190556 | 1990-10-03
(18 rows)

map=# SELECT name, population FROM country ORDER BY population DESC LIMIT 3;
    name    | population 
------------+------------
 Germany    |   83190556
 France     |   67413000
 Kazakhstan |   18711560
(3 rows)

map=# SELECT city.name, country.name, country.population FROM country
map-# JOIN city ON country.id = city.country_id
map-# WHERE city.is_capital =TRUE
map-# ORDER BY country.population DESC
map-# LIMIT 3;
    name    |    name    | population 
------------+------------+------------
 Berlin     | Germany    |   83190556
 Paris      | France     |   67413000
 Nur-Sultan | Kazakhstan |   18711560
(3 rows)

map=# SELECT * FROM country;
 id |    name    | population | last_status_change 
----+------------+------------+--------------------
  1 | Germany    |   83190556 | 1990-10-03
  2 | France     |   67413000 | 1958-10-04
  3 | Namibia    |    2550226 | 1990-03-21
  4 | Uruguay    |    3518552 | 1830-07-18
  5 | Kazakhstan |   18711560 | 1995-08-30
(5 rows)

map=# DELETE germany form country;
ERROR:  syntax error at or near "germany"
LINE 1: DELETE germany form country;
               ^
map=# DELETE Germany form country;
ERROR:  syntax error at or near "Germany"
LINE 1: DELETE Germany form country;
               ^
map=# DELETE Germany FROM country;
ERROR:  syntax error at or near "Germany"
LINE 1: DELETE Germany FROM country;
               ^
map=# DELETE FROM country WHERE id = 1;
ERROR:  update or delete on table "country" violates foreign key constraint "city_country_id_fkey" on table "city"
DETAIL:  Key (id)=(1) is still referenced from table "city".
map=# DELETE FROM country WHERE id = 1 ON DELETE SET CASCADE;
ERROR:  syntax error at or near "ON"
LINE 1: DELETE FROM country WHERE id = 1 ON DELETE SET CASCADE;
                                         ^
map=# ALTER TABLE city
map-# ADD CONTRAINT city_country_id_fkey
map-# FOREIGN KEY (country_id)
map-# REFERENCES country(id)
map-# ON DELETE SET NULL;
ERROR:  syntax error at or near "FOREIGN"
LINE 3: FOREIGN KEY (country_id)
        ^
map=# ALTER TABLE city
ADD CONSTRAINT city_country_id_fkey
FOREIGN KEY (country_id)
REFERENCES country(id)
ON DELETE SET NULL;
ERROR:  constraint "city_country_id_fkey" for relation "city" already exists
map=# ALTER TABLE city
map-# DROP CONSTRAINT city_country_id_fkey;
ALTER TABLE
map=# ALTER TABLE city
map-# ADD CONSTRAINT city_country_id_fkey
map-# FOREIGN KEY (country_id)
map-# REFERENCES country(id)
map-# ON DELETE SET NULL;
ALTER TABLE
map=# DELETE FROM country WHERE id = i;
ERROR:  column "i" does not exist
LINE 1: DELETE FROM country WHERE id = i;
                                       ^
map=# DELETE FROM country WHERE id = 1;
DELETE 1
map=# SELECT * FROM city;
 id |    name    |  area  | is_capital | country_id 
----+------------+--------+------------+------------
  1 | Nur-Sultan |  810.2 | t          |          5
  2 | Montevideo |    201 | t          |          4
  3 | Florida    |    8.2 | f          |          4
  4 | Windhoek   |   5133 | t          |          3
  5 | Swakopmund |  196.3 | f          |          3
  6 | Marseille  | 240.62 | f          |          2
  8 | Salto      |   14.2 | f          |          4
  9 | Rio Negro  |    9.3 | f          |          4
 10 | Maldonado  |    4.8 | f          |          4
 11 | Flores     |    5.1 | f          |          4
 12 | Paris      |  105.4 | t          |          2
 13 | Lyon       |  47.87 | f          |          2
 14 | Toulouse   |  118.3 | f          |          2
 15 | Nice       |  71.92 | f          |          2
  7 | Berlin     |  891.7 | t          |           
 16 | Hamburg    | 755.22 | f          |           
 17 | Munich     | 310.71 | f          |           
 18 | Frankfurt  | 248.31 | f          |           
(18 rows)

map=# SELECT * FROM country;
 id |    name    | population | last_status_change 
----+------------+------------+--------------------
  2 | France     |   67413000 | 1958-10-04
  3 | Namibia    |    2550226 | 1990-03-21
  4 | Uruguay    |    3518552 | 1830-07-18
  5 | Kazakhstan |   18711560 | 1995-08-30
(4 rows)

map=# ALTER TABLE country ADD COLUMN(code VALCHAR(5) CONSTRAINT UNIQUE);
ERROR:  syntax error at or near "("
LINE 1: ALTER TABLE country ADD COLUMN(code VALCHAR(5) CONSTRAINT UN...
                                      ^
map=# ALTER TABLE country ADD COLUMN code VARCHAR(5) UNIQUE;
ALTER TABLE
map=# \i country.sql
INSERT 0 8
map=# SELECT * FROM country
map-# ;
 id |    name     | population | last_status_change | code 
----+-------------+------------+--------------------+------
  2 | France      |   67413000 | 1958-10-04         | 
  3 | Namibia     |    2550226 | 1990-03-21         | 
  4 | Uruguay     |    3518552 | 1830-07-18         | 
  5 | Kazakhstan  |   18711560 | 1995-08-30         | 
  6 | Germany     |   83190556 | 1990-10-03         | DE
  7 | France      |   67413000 | 1958-10-04         | FR
  8 | Namibia     |    2550226 | 1990-03-21         | NA
  9 | Uruguay     |    3518552 | 1830-07-18         | UY
 10 | Kazakhstan  |   18711560 | 1995-08-30         | KZ
 11 | Spain       |   47450795 | 1986-01-01         | ES
 12 | Switzerland |    8570146 | 1848-09-12         | CH
 13 | Austria     |    8935112 | 1995-01-01         | AT
(12 rows)

map=# SELECT * FROM country WHERE code;
ERROR:  argument of WHERE must be type boolean, not type character varying
LINE 1: SELECT * FROM country WHERE code;
                                    ^
map=# SELECT * FROM country WHERE id > 5;
 id |    name     | population | last_status_change | code 
----+-------------+------------+--------------------+------
  6 | Germany     |   83190556 | 1990-10-03         | DE
  7 | France      |   67413000 | 1958-10-04         | FR
  8 | Namibia     |    2550226 | 1990-03-21         | NA
  9 | Uruguay     |    3518552 | 1830-07-18         | UY
 10 | Kazakhstan  |   18711560 | 1995-08-30         | KZ
 11 | Spain       |   47450795 | 1986-01-01         | ES
 12 | Switzerland |    8570146 | 1848-09-12         | CH
 13 | Austria     |    8935112 | 1995-01-01         | AT
(8 rows)

map=# CREATE TABLE language(code VARCHAR(5) PRIMARY KEY, name VARCHAR(20));
CREATE TABLE
map=# \i langau

map=# \i langau

map=# \i language.sql
INSERT 0 9
map=# SELECT * FROM language 
map-# ;
 code |   name    
------+-----------
 en   | English
 de   | German
 fr   | French
 af   | Afrikaans
 es   | Spanish
 kk   | Kazakh
 ru   | Russian
 it   | Italian
 ca   | Catalan
(9 rows)

map=# \i language_text.sql
language_text.sql: No such file or directory
map=# \i language_test.sql
psql:language_test.sql:1: ERROR:  duplicate key value violates unique constraint "language_pkey"
DETAIL:  Key (code)=(de) already exists.
map=# CREATE TABLE locale (name VARCHAR(20), language_code CHAR(2), country_code CHAR(2), (language_code + country_code) CONSTRAINT UNIQUE);
ERROR:  syntax error at or near "("
LINE 1: ...20), language_code CHAR(2), country_code CHAR(2), (language_...
                                                             ^
map=# CREATE TABLE locale (name VARCHAR(50), language_code CHAR(2), country_code CHAR(2), UNIQUE (language_code, country_code));
CREATE TABLE
map=# \i locale_test.sql
INSERT 0 1
map=# SELECT * FROM locale;
     name     | language_code | country_code 
--------------+---------------+--------------
 Test primary | de            | DE
(1 row)

map=# DROP TABLE locale;
DROP TABLE
map=# CREATE TABLE locale(name VARCHAR(50), language_code CHAR(2), country_code CHAR(2), UNIQUE(language_code, country_code));
CREATE TABLE
map=# \i locale.sql
INSERT 0 13
map=# SELECT * FORM locale;
ERROR:  syntax error at or near "FORM"
LINE 1: SELECT * FORM locale;
                 ^
map=# SELECT * FROM locale;
       name        | language_code | country_code 
-------------------+---------------+--------------
 German            | de            | DE
 Austrian          | de            | AT
 Swiss german      | de            | CH
 French            | fr            | FR
 Afrikaans         | af            | NA
 English (Namibia) | en            | NA
 LATAM Spanish     | es            | UY
 Spanish           | es            | ES
 Kazakh            | kk            | KZ
 Russian           | ru            | KZ
 Italian           | it            | CH
 French            | fr            | CH
 Catalan           | ca            | ES
(13 rows)

map=# \i locale_test.sql
psql:locale_test.sql:1: ERROR:  duplicate key value violates unique constraint "locale_language_code_country_code_key"
DETAIL:  Key (language_code, country_code)=(de, DE) already exists.
map=# SELECT * FROM locale;
       name        | language_code | country_code 
-------------------+---------------+--------------
 German            | de            | DE
 Austrian          | de            | AT
 Swiss german      | de            | CH
 French            | fr            | FR
 Afrikaans         | af            | NA
 English (Namibia) | en            | NA
 LATAM Spanish     | es            | UY
 Spanish           | es            | ES
 Kazakh            | kk            | KZ
 Russian           | ru            | KZ
 Italian           | it            | CH
 French            | fr            | CH
 Catalan           | ca            | ES
(13 rows)

map=# SELECT * FROM language;
 code |   name    
------+-----------
 en   | English
 de   | German
 fr   | French
 af   | Afrikaans
 es   | Spanish
 kk   | Kazakh
 ru   | Russian
 it   | Italian
 ca   | Catalan
(9 rows)

map=# SELECT * FROM country;
 id |    name     | population | last_status_change | code 
----+-------------+------------+--------------------+------
  2 | France      |   67413000 | 1958-10-04         | 
  3 | Namibia     |    2550226 | 1990-03-21         | 
  4 | Uruguay     |    3518552 | 1830-07-18         | 
  5 | Kazakhstan  |   18711560 | 1995-08-30         | 
  6 | Germany     |   83190556 | 1990-10-03         | DE
  7 | France      |   67413000 | 1958-10-04         | FR
  8 | Namibia     |    2550226 | 1990-03-21         | NA
  9 | Uruguay     |    3518552 | 1830-07-18         | UY
 10 | Kazakhstan  |   18711560 | 1995-08-30         | KZ
 11 | Spain       |   47450795 | 1986-01-01         | ES
 12 | Switzerland |    8570146 | 1848-09-12         | CH
 13 | Austria     |    8935112 | 1995-01-01         | AT
(12 rows)

map=# SELECT locale.name AS Locale, country.name AS Language, country.name AS country
map-# FROM locale JOIN language, country ON country.code = locale.country_code, locale.language_code = language.code;
ERROR:  syntax error at or near ","
LINE 2: FROM locale JOIN language, country ON country.code = locale....
                                 ^
map=# SELECT locale.name AS Locale, country.name AS Language, country.name AS country
map-# FROM locale
map-# JOIN country ON locale.country_code = country.code
map-# JOIN language ON locale.language_code = language.code;
      locale       |  language   |   country   
-------------------+-------------+-------------
 English (Namibia) | Namibia     | Namibia
 Swiss german      | Switzerland | Switzerland
 Austrian          | Austria     | Austria
 German            | Germany     | Germany
 French            | Switzerland | Switzerland
 French            | France      | France
 Afrikaans         | Namibia     | Namibia
 Spanish           | Spain       | Spain
 LATAM Spanish     | Uruguay     | Uruguay
 Kazakh            | Kazakhstan  | Kazakhstan
 Russian           | Kazakhstan  | Kazakhstan
 Italian           | Switzerland | Switzerland
 Catalan           | Spain       | Spain
(13 rows)

map=# SELECT * FROM country;
 id |    name     | population | last_status_change | code 
----+-------------+------------+--------------------+------
  2 | France      |   67413000 | 1958-10-04         | 
  3 | Namibia     |    2550226 | 1990-03-21         | 
  4 | Uruguay     |    3518552 | 1830-07-18         | 
  5 | Kazakhstan  |   18711560 | 1995-08-30         | 
  6 | Germany     |   83190556 | 1990-10-03         | DE
  7 | France      |   67413000 | 1958-10-04         | FR
  8 | Namibia     |    2550226 | 1990-03-21         | NA
  9 | Uruguay     |    3518552 | 1830-07-18         | UY
 10 | Kazakhstan  |   18711560 | 1995-08-30         | KZ
 11 | Spain       |   47450795 | 1986-01-01         | ES
 12 | Switzerland |    8570146 | 1848-09-12         | CH
 13 | Austria     |    8935112 | 1995-01-01         | AT
(12 rows)

map=# SELECT * FROM locale;
       name        | language_code | country_code 
-------------------+---------------+--------------
 German            | de            | DE
 Austrian          | de            | AT
 Swiss german      | de            | CH
 French            | fr            | FR
 Afrikaans         | af            | NA
 English (Namibia) | en            | NA
 LATAM Spanish     | es            | UY
 Spanish           | es            | ES
 Kazakh            | kk            | KZ
 Russian           | ru            | KZ
 Italian           | it            | CH
 French            | fr            | CH
 Catalan           | ca            | ES
(13 rows)

map=# SELECT * FROM langauage;
ERROR:  relation "langauage" does not exist
LINE 1: SELECT * FROM langauage;
                      ^
map=# SELECT * FROM language;
 code |   name    
------+-----------
 en   | English
 de   | German
 fr   | French
 af   | Afrikaans
 es   | Spanish
 kk   | Kazakh
 ru   | Russian
 it   | Italian
 ca   | Catalan
(9 rows)

map=# SELECT locale.name AS Locale, country.name AS Country, language.name AS Language
map-# FROM lacale JOIN country ON locale.country_code = country.code
map-# JOIN language ON locale.language_code = language.code
map-# ORDER BY DESC;
ERROR:  syntax error at or near "DESC"
LINE 4: ORDER BY DESC;
                 ^
map=# SELECT locale.name AS Locale, language.name AS Language, country.name AS Country
map-# FROM locale JOIN country ON locale.country_code = country.code
map-# JOIN language ON locale.language_code = language.code
map-# ORDER BY language.name ABC;
ERROR:  syntax error at or near "ABC"
LINE 4: ORDER BY language.name ABC;
                               ^
map=# SELECT locale.name AS Locale, language.name AS Language, country.name AS Country
FROM locale JOIN country ON locale.country_code = country.code
JOIN language ON locale.language_code = language.code
ORDER BY language.name ASC;
      locale       | language  |   country   
-------------------+-----------+-------------
 Afrikaans         | Afrikaans | Namibia
 Catalan           | Catalan   | Spain
 English (Namibia) | English   | Namibia
 French            | French    | France
 French            | French    | Switzerland
 Austrian          | German    | Austria
 German            | German    | Germany
 Swiss german      | German    | Switzerland
 Italian           | Italian   | Switzerland
 Kazakh            | Kazakh    | Kazakhstan
 Russian           | Russian   | Kazakhstan
 LATAM Spanish     | Spanish   | Uruguay
 Spanish           | Spanish   | Spain
(13 rows)

map=# 
