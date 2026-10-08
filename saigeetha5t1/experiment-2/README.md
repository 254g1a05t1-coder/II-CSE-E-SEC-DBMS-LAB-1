# DBMS LAB - WEEK 2

## Sailors, Boats and Reserves

### Table Creation

```sql
CREATE TABLE Sailors (
    sid NUMBER PRIMARY KEY,
    sname VARCHAR2(50) NOT NULL,
    rating NUMBER NOT NULL,
    age NUMBER(4,1) NOT NULL
);
![Output 1](output1.png)
```
CREATE TABLE Boats (
    bid NUMBER PRIMARY KEY,
    bname VARCHAR2(20) NOT NULL,
    color VARCHAR2(10) NOT NULL
);
![Output 1](output2.png)
````
CREATE TABLE Reserves (
    sid NUMBER NOT NULL,
    bid NUMBER NOT NULL,
    day DATE NOT NULL,
    PRIMARY KEY (sid, bid, day),
    FOREIGN KEY (sid) REFERENCES Sailors(sid),
    FOREIGN KEY (bid) REFERENCES Boats(bid)
);
```
![Output 3](output3.png)
```
SELECT * FROM tab;
````
##  insert into boats
INSERT INTO Boats
VALUES(22,'Dustin',7,45.0);
## insert into sailors 
INSERT INTO Sailors VALUES (22, 'Dustin', 7, 45.0);
INSERT INTO Sailors VALUES (29, 'Brutus', 1, 33.0);
INSERT INTO Sailors VALUES (31, 'Lubber', 8, 55.5);
INSERT INTO Sailors VALUES (32, 'Andy', 8, 25.5);
INSERT INTO Sailors VALUES (58, 'Rusty', 10, 35.0);
INSERT INTO Sailors VALUES (64, 'Horatio', 7, 35.0);
INSERT INTO Sailors VALUES (71, 'Zorba', 10, 16.0);
INSERT INTO Sailors VALUES (74, 'Horatio', 9, 35.0);
INSERT INTO Sailors VALUES (85, 'Art', 3, 25.5);
INSERT INTO Sailors VALUES (95, 'Bob', 3, 63.5);
## insert into reserves 
INSERT INTO Reserves VALUES (22, 101, TO_DATE('10/10/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (22, 102, TO_DATE('10/10/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (22, 103, TO_DATE('10/8/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (22, 104, TO_DATE('10/7/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (31, 102, TO_DATE('11/10/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (31, 103, TO_DATE('11/6/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (31, 104, TO_DATE('11/12/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (31, 104, TO_DATE('11/12/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (64, 101, TO_DATE('9/5/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (64, 102, TO_DATE('9/8/98','MM/DD/RR'));
INSERT INTO Reserves VALUES (74, 103, TO_DATE('9/8/98','MM/DD/RR'));
## insert into boats
INSERT INTO Boats VALUES (101, 'Interlake', 'blue');
INSERT INTO Boats VALUES (102, 'Interlake', 'red');
INSERT INTO Boats VALUES (103, 'Clipper', 'green');
INSERT INTO Boats VALUES (104, 'Marine', 'red');
## desc tables
DESC sailors;
DESC reserves;
DESC boats;
## display 
SELECT * FROM Sailors;
SELECT * FROM Reserves;
SELECT * FROM Boats;
``
# DBMS LAB - EXPERIMENT 2

## Query 1

```sql
SELECT sname,age FROM Sailors;
```

![Output](q1.png)

---

## Query 2

```sql
SELECT sname FROM Sailors WHERE rating>7;
```

![Output](q2.png)

---

## Query 3

```sql
SELECT s.sname FROM Sailors s,Reserves r
WHERE s.sid=r.sid
AND r.bid=103;
```

![Output](q3.png)

---

## Query 4

```sql
SELECT DISTINCT r.sid
FROM Reserves r,Boats b
WHERE r.bid=b.bid
AND b.color='red';
```

![Output](q4.png)

---

## Query 5

```sql
SELECT DISTINCT s.sname FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND b.color='red';
```

![Output](q5.png)

---

## Query 6

```sql
SELECT b.color FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND s.sname='Lubber';
```

![Output](q6.png)

---

## Query 7

```sql
SELECT DISTINCT s.sname FROM Sailors s,Reserves r
WHERE s.sid=r.sid;
```

![Output](q7.png)

---

## Query 8

```sql
SELECT DISTINCT s.sname,rating+1 AS incremented_rating
FROM Sailors s,Reserves r1,Reserves r2
WHERE s.sid=r1.sid AND r1.sid=r2.sid
AND r2.day=r2.day AND r1.bid < > r2.bid;
```

![Output](q8.png)

---

## Query 9

```sql
SELECT age
FROM Sailors
WHERE sname LIKE 'B%b'
AND LENGTH(sname) >= 3;
```

![Output](q9.png)

---

## Query 10

```sql
SELECT s.sname FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND(b.color='red' OR b.color='green');
```

![Output](q10.png)

---

## Query 11

```sql
SELECT s.sname
FROM Sailors s
WHERE s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'red'
)
AND s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'green'
);
```

![Output](q11.png)

---

## Query 12

```sql
SELECT DISTINCT r.sid
FROM Reserves r, Boats b
WHERE r.bid = b.bid
AND b.color = 'red'
AND r.sid NOT IN
(
    SELECT r2.sid
    FROM Reserves r2, Boats b2
    WHERE r2.bid = b2.bid
    AND b2.color = 'green'
);
```

![Output](q12.png)

---

## Query 13

```sql
SELECT sid
FROM Sailors
WHERE rating = 10

UNION

SELECT sid
FROM Reserves
WHERE bid = 104;
```

![Output](q13.png)

---

## Query 14

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND r.bid = 103;
```

![Output](q14.png)

---

## Query 15

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r, Boats b
WHERE s.sid = r.sid
AND r.bid = b.bid
AND b.color = 'red';
```

![Output](q15.png)

---

## Query 16

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND r.bid = 103;
```

![Output](q16.png)

---

## Query 17

```sql
SELECT *
FROM Sailors
WHERE rating > ANY
(
    SELECT rating
    FROM Sailors
    WHERE sname = 'Horatio'
);
```

![Output](q17.png)

---

## Query 18

```sql
SELECT *
FROM Sailors
WHERE rating > ALL
(
    SELECT rating
    FROM Sailors
    WHERE sname = 'Horatio'
);
```

![Output](q18.png)

---

## Query 19

```sql
SELECT * FROM Sailors
WHERE rating =
(
    SELECT MAX(rating)
    FROM Sailors
);
```

![Output](q19.png)

---

## Query 20

```sql
SELECT s.sname
FROM Sailors s
WHERE s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'red'
)
AND s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'green'
);
```

![Output](q20.png)

---

## Query 21

```sql
SELECT s.sname FROM Sailors s
WHERE NOT EXISTS
(
    SELECT * FROM Boats b
    WHERE NOT EXISTS
    (
        SELECT *
        FROM Reserves r
        WHERE r.sid = s.sid
        AND r.bid = b.bid
    )
);
```

![Output](q21.png)

---

## Query 22

```sql
SELECT AVG(age)
FROM Sailors;
```

![Output](q22.png)

---

## Query 23

```sql
SELECT AVG(age)
FROM Sailors
```

![Output](q23.png)

---

## Query 24

```sql
SELECT sname, age
FROM Sailors
WHERE age =
(
    SELECT MAX(age)
);
```

![Output](q24.png)

---

## Query 25

```sql
SELECT COUNT(*)
FROM Sailors;
```

![Output](q25.png)

---

## Query 26

```sql
SELECT COUNT(DISTINCT sname)
FROM Sailors;
```

![Output](q26.png)

---

## Query 27

```sql
SELECT sname
FROM Sailors
WHERE age >
(
    SELECT MAX(age)
    FROM Sailors
    WHERE rating = 10
);
```

![Output](q27.png)

---

## Query 28

```sql
SELECT rating, MIN(age)
FROM Sailors
GROUP BY rating;
```

![Output](q28.png)

---

## Query 29

```sql
SELECT rating, MIN(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q29.png)

---

## Query 30

```sql
SELECT b.bid, COUNT(r.sid) AS reservations
FROM Boats b
LEFT JOIN Reserves r
ON b.bid = r.bid
WHERE b.color = 'red'
GROUP BY b.bid;
```

![Output](q30.png)

---

## Query 31

```sql
SELECT rating, AVG(age)
FROM Sailors
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q31.png)

---

## Query 32

```sql
SELECT rating, AVG(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q32.png)

---

## Query 33

```sql
SELECT rating, AVG(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q33.png)

---

## Query 34

```sql
SELECT rating
FROM Sailors
GROUP BY rating
HAVING AVG(age) =
(
    SELECT MIN(avg_age)
    FROM
    (
        SELECT AVG(age) AS avg_age
        FROM Sailors
        GROUP BY rating
    ) x
);
```
# DBMS LAB - EXPERIMENT 2

## Query 1

```sql
SELECT sname,age FROM Sailors;
```

![Output](q1.png)

---

## Query 2

```sql
SELECT sname FROM Sailors WHERE rating>7;
```

![Output](q2.png)

---

## Query 3

```sql
SELECT s.sname FROM Sailors s,Reserves r
WHERE s.sid=r.sid
AND r.bid=103;
```

![Output](q3.png)

---

## Query 4

```sql
SELECT DISTINCT r.sid
FROM Reserves r,Boats b
WHERE r.bid=b.bid
AND b.color='red';
```

![Output](q4.png)

---

## Query 5

```sql
SELECT DISTINCT s.sname FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND b.color='red';
```

![Output](q5.png)

---

## Query 6

```sql
SELECT b.color FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND s.sname='Lubber';
```

![Output](q6.png)

---

## Query 7

```sql
SELECT DISTINCT s.sname FROM Sailors s,Reserves r
WHERE s.sid=r.sid;
```

![Output](q7.png)

---

## Query 8

```sql
SELECT DISTINCT s.sname,rating+1 AS incremented_rating
FROM Sailors s,Reserves r1,Reserves r2
WHERE s.sid=r1.sid AND r1.sid=r2.sid
AND r2.day=r2.day AND r1.bid < > r2.bid;
```

![Output](q8.png)

---

## Query 9

```sql
SELECT age
FROM Sailors
WHERE sname LIKE 'B%b'
AND LENGTH(sname) >= 3;
```

![Output](q9.png)

---

## Query 10

```sql
SELECT s.sname FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND(b.color='red' OR b.color='green');
```

![Output](q10.png)

---

## Query 11

```sql
SELECT s.sname
FROM Sailors s
WHERE s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'red'
)
AND s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'green'
);
```

![Output](q11.png)

---

## Query 12

```sql
SELECT DISTINCT r.sid
FROM Reserves r, Boats b
WHERE r.bid = b.bid
AND b.color = 'red'
AND r.sid NOT IN
(
    SELECT r2.sid
    FROM Reserves r2, Boats b2
    WHERE r2.bid = b2.bid
    AND b2.color = 'green'
);
```

![Output](q12.png)

---

## Query 13

```sql
SELECT sid
FROM Sailors
WHERE rating = 10

UNION

SELECT sid
FROM Reserves
WHERE bid = 104;
```

![Output](q13.png)

---

## Query 14

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND r.bid = 103;
```

![Output](q14.png)

---

## Query 15

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r, Boats b
WHERE s.sid = r.sid
AND r.bid = b.bid
AND b.color = 'red';
```

![Output](q15.png)

---

## Query 16

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND r.bid = 103;
```

![Output](q16.png)

---

## Query 17

```sql
SELECT *
FROM Sailors
WHERE rating > ANY
(
    SELECT rating
    FROM Sailors
    WHERE sname = 'Horatio'
);
```

![Output](q17.png)

---

## Query 18

```sql
SELECT *
FROM Sailors
WHERE rating > ALL
(
    SELECT rating
    FROM Sailors
    WHERE sname = 'Horatio'
);
```

![Output](q18.png)

---

## Query 19

```sql
SELECT * FROM Sailors
WHERE rating =
(
    SELECT MAX(rating)
    FROM Sailors
);
```

![Output](q19.png)

---

## Query 20

```sql
SELECT s.sname
FROM Sailors s
WHERE s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'red'
)
AND s.sid IN
(
    SELECT r.sid
    FROM Reserves r, Boats b
    WHERE r.bid = b.bid
    AND b.color = 'green'
);
```

![Output](q20.png)

---

## Query 21

```sql
SELECT s.sname FROM Sailors s
WHERE NOT EXISTS
(
    SELECT * FROM Boats b
    WHERE NOT EXISTS
    (
        SELECT *
        FROM Reserves r
        WHERE r.sid = s.sid
        AND r.bid = b.bid
    )
);
```

![Output](q21.png)

---

## Query 22

```sql
SELECT AVG(age)
FROM Sailors;
```

![Output](q22.png)

---

## Query 23

```sql
SELECT AVG(age)
FROM Sailors
```

![Output](q23.png)

---

## Query 24

```sql
SELECT sname, age
FROM Sailors
WHERE age =
(
    SELECT MAX(age)
);
```

![Output](q24.png)

---

## Query 25

```sql
SELECT COUNT(*)
FROM Sailors;
```

![Output](q25.png)

---

## Query 26

```sql
SELECT COUNT(DISTINCT sname)
FROM Sailors;
```

![Output](q26.png)

---

## Query 27

```sql
SELECT sname
FROM Sailors
WHERE age >
(
    SELECT MAX(age)
    FROM Sailors
    WHERE rating = 10
);
```

![Output](q27.png)

---

## Query 28

```sql
SELECT rating, MIN(age)
FROM Sailors
GROUP BY rating;
```

![Output](q28.png)

---

## Query 29

```sql
SELECT rating, MIN(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q29.png)

---

## Query 30

```sql
SELECT b.bid, COUNT(r.sid) AS reservations
FROM Boats b
LEFT JOIN Reserves r
ON b.bid = r.bid
WHERE b.color = 'red'
GROUP BY b.bid;
```

![Output](q30.png)

---

## Query 31

```sql
SELECT rating, AVG(age)
FROM Sailors
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q31.png)

---

## Query 32

```sql
SELECT rating, AVG(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q32.png)

---

## Query 33

```sql
SELECT rating, AVG(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Output](q33.png)

---

## Query 34

```sql
SELECT rating
FROM Sailors
GROUP BY rating
HAVING AVG(age) =
(
    SELECT MIN(avg_age)
    FROM
    (
        SELECT AVG(age) AS avg_age
        FROM Sailors
        GROUP BY rating
    ) x
);

 ```


![Output](q34.png)
