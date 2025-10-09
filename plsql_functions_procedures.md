# Sports Management System – Functions & Procedures

## 1. Introduction

This project demonstrates the use of **functions and stored procedures** in a **Sports Management System**.  
The system tracks players, matches, and individual player scores. Key concepts include:

- **User-Defined Functions (UDFs)**: To calculate player initials or total points.  
- **Stored Procedures**: To perform operations like inserting players and completing matches.  
- **Use Case**: Managing a sports club's database efficiently using SQL.

---

## 2. Database Design

### 2.1 Database: `sports_club`
```sql
CREATE DATABASE IF NOT EXISTS sports_club;
USE sports_club;
```

### 2.2 Tables

#### 2.2.1 `players` Table
Stores information about each player.
```sql
CREATE TABLE players (
    player_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT NOT NULL,
    sport VARCHAR(30) NOT NULL,
    team VARCHAR(30),
    email VARCHAR(100) UNIQUE
);
```
**Sample Data:**
```sql
INSERT INTO players (name, age, sport, team, email) VALUES
('Rutuja Gholap', 22, 'Cross country', 'Team A', 'rutuja@gmail.com'),
('Vrunda Batwal', 21, '1500 m', 'Team B', 'vrunda@gmail.com'),
('Vedant Bhor', 19, 'Cricket', 'Team C', 'vedant@gmail.com');
```
**Output:**
| player_id | name           | age | sport         | team   | email             |
|-----------|----------------|-----|---------------|--------|------------------|
| 1         | Rutuja Gholap  | 22  | Cross country | Team A | rutuja@gmail.com |
| 2         | Vrunda Batwal  | 21  | 1500 m        | Team B | vrunda@gmail.com |
| 3         | Vedant Bhor    | 19  | Cricket       | Team C | vedant@gmail.com |

---

#### 2.2.2 `matches` Table
Stores match details.
```sql
CREATE TABLE matches (
    match_id INT AUTO_INCREMENT PRIMARY KEY,
    match_date DATE NOT NULL,
    sport VARCHAR(30) NOT NULL,
    team1 VARCHAR(30) NOT NULL,
    team2 VARCHAR(30) NOT NULL,
    score_team1 INT DEFAULT 0,
    score_team2 INT DEFAULT 0,
    status VARCHAR(20) DEFAULT 'SCHEDULED'
);
```
**Sample Data:**
```sql
INSERT INTO matches (match_date, sport, team1, team2) VALUES
('2025-10-01', 'Cross country', 'Team A', 'Team B'),
('2025-10-05', '1500 m', 'Team B', 'Team C'),
('2025-10-08', 'Cricket', 'Team C', 'Team A');
```
**Output:**
| match_id | match_date | sport         | team1  | team2  | score_team1 | score_team2 | status    |
|----------|------------|---------------|--------|--------|-------------|-------------|-----------|
| 1        | 2025-10-01 | Cross country | Team A | Team B | 0           | 0           | SCHEDULED |
| 2        | 2025-10-05 | 1500 m        | Team B | Team C | 0           | 0           | SCHEDULED |
| 3        | 2025-10-08 | Cricket       | Team C | Team A | 0           | 0           | SCHEDULED |

---

#### 2.2.3 `scores` Table
Stores player points per match.
```sql
CREATE TABLE scores (
    score_id INT AUTO_INCREMENT PRIMARY KEY,
    player_id INT NOT NULL,
    match_id INT NOT NULL,
    points INT NOT NULL,
    FOREIGN KEY (player_id) REFERENCES players(player_id),
    FOREIGN KEY (match_id) REFERENCES matches(match_id)
);
```
**Sample Data:**
```sql
INSERT INTO scores (player_id, match_id, points) VALUES
(1, 1, 15),
(2, 1, 10),
(2, 2, 20),
(3, 2, 25),
(3, 3, 30),
(1, 3, 35);
```
**Output:**
| score_id | player_id | match_id | points |
|----------|-----------|----------|--------|
| 1        | 1         | 1        | 15     |
| 2        | 2         | 1        | 10     |
| 3        | 2         | 2        | 20     |
| 4        | 3         | 2        | 25     |
| 5        | 3         | 3        | 30     |
| 6        | 1         | 3        | 35     |

---

## 3. User-Defined Functions (UDFs)

### 3.1 Get Player Initials
```sql
DELIMITER //
CREATE FUNCTION get_player_initials(p_name VARCHAR(100))
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    RETURN CONCAT(
        LEFT(SUBSTRING_INDEX(p_name, ' ', 1), 1),
        LEFT(SUBSTRING_INDEX(p_name, ' ', -1), 1)
    );
END //
DELIMITER ;
```
**Usage:**
```sql
SELECT name, get_player_initials(name) AS initials FROM players;
```
**Output:**
| name           | initials |
|----------------|----------|
| Rutuja Gholap  | RG       |
| Vrunda Batwal  | VB       |
| Vedant Bhor    | VB       |

---

### 3.2 Get Player Total Points
```sql
DELIMITER //
CREATE FUNCTION get_player_total_points(p_id INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE total_points INT;
    SELECT SUM(points) INTO total_points
    FROM scores
    WHERE player_id = p_id;
    RETURN total_points;
END //
DELIMITER ;
```
**Usage:**
```sql
SELECT name, get_player_total_points(player_id) AS total_points FROM players;
```
**Output:**
| name           | total_points |
|----------------|--------------|
| Rutuja Gholap  | 50           |
| Vrunda Batwal  | 30           |
| Vedant Bhor    | 55           |

---

## 4. Stored Procedures

### 4.1 Insert New Player
```sql
DELIMITER //
CREATE PROCEDURE insert_player(
    IN p_name VARCHAR(50),
    IN p_age INT,
    IN p_sport VARCHAR(30),
    IN p_team VARCHAR(30),
    IN p_email VARCHAR(100)
)
BEGIN
    INSERT INTO players (name, age, sport, team, email)
    VALUES (p_name, p_age, p_sport, p_team, p_email);
END //
DELIMITER ;
```
**Usage:**
```sql
CALL insert_player('Bhakti Tajne', 21, 'Cross country', 'Team A', 'bhakti@gmail.com');
```
**Output:**
| player_id | name          | age | sport         | team   | email            |
|-----------|---------------|-----|---------------|--------|-----------------|
| 4         | Bhakti Tajne  | 21  | Cross country | Team A | bhakti@gmail.com |

---

### 4.2 Complete a Match
```sql
DELIMITER //
CREATE PROCEDURE complete_match(
    IN p_match_id INT,
    IN p_score_team1 INT,
    IN p_score_team2 INT
)
BEGIN
    UPDATE matches
    SET score_team1 = p_score_team1,
        score_team2 = p_score_team2,
        status = 'COMPLETED'
    WHERE match_id = p_match_id;
END //
DELIMITER ;
```
**Usage:**
```sql
CALL complete_match(1, 3, 2);
SELECT * FROM matches WHERE match_id = 1;
```
**Output:**
| match_id | match_date | sport         | team1  | team2  | score_team1 | score_team2 | status    |
|----------|------------|---------------|--------|--------|-------------|-------------|-----------|
| 1        | 2025-10-01 | Cross country | Team A | Team B | 3           | 2           | COMPLETED |

---

### 4.3 Player Total Points (OUT Parameter)
```sql
DELIMITER //
CREATE PROCEDURE player_total_points(
    IN p_player_id INT,
    OUT p_total INT
)
BEGIN
    SELECT SUM(points) INTO p_total
    FROM scores
    WHERE player_id = p_player_id;
END //
DELIMITER ;
```
**Usage:**
```sql
CALL player_total_points(1, @total);
SELECT @total;
```
**Output:**
| @total |
|--------|
| 50     |

---

## 5. Summary

- Created a **Sports Management Database** with `players`, `matches`, and `scores` tables.  
- Implemented **user-defined functions** to get player initials and total points.  
- Implemented **stored procedures** to insert players, complete matches, and calculate total points.  
- Demonstrated **usage with input and output examples**.

