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

CREATE TABLE Boats (
    bid NUMBER PRIMARY KEY,
    bname VARCHAR2(20) NOT NULL,
    color VARCHAR2(10) NOT NULL
);

CREATE TABLE Reserves (
    sid NUMBER NOT NULL,
    bid NUMBER NOT NULL,
    day DATE NOT NULL,
    PRIMARY KEY (sid, bid, day),
    FOREIGN KEY (sid) REFERENCES Sailors(sid),
    FOREIGN KEY (bid) REFERENCES Boats(bid)
);
```

### Query 1

```sql
SELECT sname,age FROM Sailors;
```

![Query 1 Output](q1.png)

### Query 2

```sql
SELECT sname FROM Sailors WHERE rating>7;
```

![Query 2 Output](q2.png)

### Query 3

```sql
SELECT s.sname FROM Sailors s,Reserves r
WHERE s.sid=r.sid
AND r.bid=103;
```

![Query 3 Output](q3.png)

### Query 4

```sql
SELECT DISTINCT r.sid
FROM Reserves r,Boats b
WHERE r.bid=b.bid
AND b.color='red';
```

![Query 4 Output](q4.png)

### Query 5

```sql
SELECT DISTINCT s.sname FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND b.color='red';
```

![Query 5 Output](q5.png)

### Query 6

```sql
SELECT b.color FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND s.sname='Lubber';
```

![Query 6 Output](q6.png)

### Query 7

```sql
SELECT DISTINCT s.sname FROM Sailors s,Reserves r
WHERE s.sid=r.sid;
```

![Query 7 Output](q7.png)

### Query 8

```sql
SELECT DISTINCT s.sname,rating+1 AS incremented_rating
FROM Sailors s,Reserves r1,Reserves r2
WHERE s.sid=r1.sid AND r1.sid=r2.sid
AND r2.day=r2.day AND r1.bid < > r2.bid;
```

![Query 8 Output](q8.png)

### Query 9

```sql
SELECT age
FROM Sailors
WHERE sname LIKE 'B%b'
AND LENGTH(sname) >= 3;
```

![Query 9 Output](q9.png)

### Query 10

```sql
SELECT s.sname FROM Sailors s,Reserves r,Boats b
WHERE s.sid=r.sid
AND r.bid=b.bid
AND(b.color='red' OR b.color='green');
```

![Query 10 Output](q10.png)

### Query 11

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

![Query 11 Output](q11.png)

### Query 12

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

![Query 12 Output](q12.png)

### Query 13

```sql
SELECT sid
FROM Sailors
WHERE rating = 10

UNION

SELECT sid
FROM Reserves
WHERE bid = 104;
```

![Query 13 Output](q13.png)

### Query 14

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND r.bid = 103;
```

![Query 14 Output](q14.png)

### Query 15

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r, Boats b
WHERE s.sid = r.sid
AND r.bid = b.bid
AND b.color = 'red';
```

![Query 15 Output](q15.png)

### Query 16

```sql
SELECT DISTINCT s.sname
FROM Sailors s, Reserves r
WHERE s.sid = r.sid
AND r.bid = 103;
```

![Query 16 Output](q16.png)

### Query 17

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

![Query 17 Output](q17.png)

### Query 18

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

![Query 18 Output](q18.png)

### Query 19

```sql
SELECT * FROM Sailors
WHERE rating =
(
    SELECT MAX(rating)
    FROM Sailors
);
```

![Query 19 Output](q19.png)

### Query 20

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

![Query 20 Output](q20.png)

### Query 21

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

![Query 21 Output](q21.png)

### Query 22

```sql
SELECT AVG(age)
FROM Sailors;
```

![Query 22 Output](q22.png)

### Query 23

```sql
SELECT AVG(age)
FROM Sailors
```

![Query 23 Output](q23.png)

### Query 24

```sql
SELECT sname, age
FROM Sailors
WHERE age =
(
    SELECT MAX(age)
);
```

![Query 24 Output](q24.png)

### Query 25

```sql
SELECT COUNT(*)
FROM Sailors;
```

![Query 25 Output](q25.png)

### Query 26

```sql
SELECT COUNT(DISTINCT sname)
FROM Sailors;
```

![Query 26 Output](q26.png)

### Query 27

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

![Query 27 Output](q27.png)

### Query 28

```sql
SELECT rating, MIN(age)
FROM Sailors
GROUP BY rating;
```

![Query 28 Output](q28.png)

### Query 29

```sql
SELECT rating, MIN(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Query 29 Output](q29.png)

### Query 30

```sql
SELECT b.bid, COUNT(r.sid) AS reservations
FROM Boats b
LEFT JOIN Reserves r
ON b.bid = r.bid
WHERE b.color = 'red'
GROUP BY b.bid;
```

![Query 30 Output](q30.png)

### Query 31

```sql
SELECT rating, AVG(age)
FROM Sailors
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Query 31 Output](q31.png)

### Query 32

```sql
SELECT rating, AVG(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Query 32 Output](q32.png)

### Query 33

```sql
SELECT rating, AVG(age)
FROM Sailors
WHERE age >= 18
GROUP BY rating
HAVING COUNT(*) >= 2;
```

![Query 33 Output](q33.png)

### Query 34

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

![Query 34 Output](q34.png)
