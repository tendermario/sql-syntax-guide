// ACTUAL NICELY FORMATTED DOC AVAILABLE HERE:
// https://docs.google.com/document/d/1SYgWD-lg5tD26FRY6WxCIT-WbKLySIUOi7y7kRGz_0k/edit#

// This is a sparknotes-version of the essentials for those that are just
// starting out with SQL that sometimes forget the basic syntax (like me)

How to Use MySQL Queries
Setup
Set up your MySQL Server (???)
Commands
What Don types when starting his SQL server
sqlite3
.mode headers
.mode column
.headers on
Starting
CREATE TABLE
CREATE TABLE user (id INTEGER PRIMARY KEY AUTOINCREMENT,
         username TEXT, ...);
Adding
INSERT INTO … VALUES
INSERT INTO user VALUES (id 1, username “Mario”);
INSERT INTO user (id, username) VALUES (1, “Mario”);
Filtering
The general syntax seems to go as follows:
SELECT your columns
FROM database table
WHERE filters the data/rows to choose
GROUP BY grouping output rows as single rows
      HAVING filters the resulting data
ORDER BY what order the rows come back in
SELECT …
SELECT * FROM user
  DISTINCT - only shows one row for each value in that column (removes duplicates)
SELECT DISTINCT yr FROM nobel
Select Functions + Modifiers
SUM(...), AVG(...), COUNT(...), ROUND(...), CONCAT(...), REPLACE(variable, ‘this’, ‘that’)
    I believe these require a “GROUP BY” to know what the function applies to
SELECT type, SUM(calories) FROM exercise_logs GROUP BY type;
… AS … - aliases the function, uses it for the column title
SELECT type, SUM(calories) AS calorie_count FROM exercise_logs GROUP BY type;
Select Creating a custom column
CASE WHEN … THEN … END - like switch or if/else to name different conditions. End is needed.
SELECT type, heart_rate,
    CASE
        WHEN heart_rate > 220-30 THEN "above max"
        WHEN heart_rate > ROUND(0.90 * (220-30)) THEN "above target"
        WHEN heart_rate > ROUND(0.50 * (220-30)) THEN "within target"
        ELSE "below target"
    END AS "hr_zone"
  Ex.2:
SELECT name, continent,
  CASE
   WHEN continent = "Oceania" THEN "Australasia"
   WHEN continent = "Eurasia" THEN "Europe/Asia"
   WHEN name = "Turkey" THEN "Europe/Asia"
   WHEN continent = "Caribbean" AND name LIKE "B%" THEN "North America"
   WHEN continent = "Caribbean" THEN "South America"
   ELSE continent END AS tld
  FROM world

FROM ... which database to select from
SELECT * FROM user
From - Joining tables
JOIN … ON ...
SELECT users.username, COUNT(posts.id) AS post_count
FROM users JOIN posts ON users.id=posts.user_id
  HAVING post_count > 10000;
INNER JOIN … ON … - only gets the results where the columns described in the equivalency have a match
  SELECT p.id, p.name, l.city
    FROM products AS p INNER JOIN locations AS l ON l.id = p.location_id
  OUTER LEFT JOIN … ON … - gets the results where the columns match + the ones that don’t match from the left (i.e.
 From products in the example)
  SELECT p.id, p.name, l.city
      FROM products AS p OUTER LEFT JOIN locations AS l ON l.id = p.location_id
  OUTER RIGHT JOIN … ON … - gets the results where the columns match + the ones that don’t match from the right
(i.e. From location in the example)
  SELECT p.id, p.name, l.city
      FROM products AS p OUTER RIGHT JOIN locations AS l ON l.id = p.location_id
WHERE …
SELECT * FROM user WHERE id = 1 OR id = 2;
IN …
SELECT * FROM user WHERE id IN (1, 2);
NOT
SELECT * FROM user WHERE id NOT IN (1, 2);
LIKE and % wildcard
SELECT * FROM user WHERE name LIKE (“%ar%”);
BETWEEN … AND …
  SELECT * FROM user WHERE id BETWEEN 0 AND 100;
GROUP BY …
SELECT type FROM exercise_logs GROUP BY type;
HAVING … - uses the function or a named variable as a filter
SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150;
ORDER BY … separate extra arguments to order by with a comma
SELECT type, SUM(calories) AS total_calories FROM exercise_logs
    GROUP BY type
    HAVING total_calories > 150;
    ORDER BY type ASC;
