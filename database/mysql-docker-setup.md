# MySQL docker setup

```bash
[docker@localhost ~]$ sudo docker pull mysql
[docker@localhost ~]$ sudo mkdir /home/docker/mysql-db
[docker@localhost ~]$ sudo docker run --name test-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw -v /home/docker/mysql-db:/var/lib/mysql -d mysql:latest
```

PL/SQL:建立資料庫和資料表

```sql
-- create database
CREATE DATABASE TestDB

-- 建立名為 Inventory 的新資料表
CREATE TABLE Inventory (SKU CHAR(5) NOT NULL, product VARCHAR(50), quantity INT, UNIQUE KEY (SKU))

-- 將資料插入新的資料表
INSERT INTO Inventory VALUES ('14256', 'banana', 150);
INSERT INTO Inventory VALUES ('24589', 'avocado', 7863783);
INSERT INTO Inventory VALUES ('35587', 'cherry', 34345345);
INSERT INTO Inventory VALUES ('55782', 'coconut', 179);
INSERT INTO Inventory VALUES ('77996', 'jujube', 14783);
INSERT INTO Inventory VALUES ('77837', 'durian', 47892);
INSERT INTO Inventory VALUES ('89653', 'grape', 258);
INSERT INTO Inventory VALUES ('98627', 'grapefruit', 47);
INSERT INTO Inventory VALUES ('87667', 'honeydew melon', 39387);
INSERT INTO Inventory VALUES ('64556', 'lemon', 39487);
INSERT INTO Inventory VALUES ('27287', 'orange', 387);
INSERT INTO Inventory VALUES ('78683', 'kiwi fruit', 337);
INSERT INTO Inventory VALUES ('12373', 'mangosteen', 797);
INSERT INTO Inventory VALUES ('36483', 'pear', 2786);
INSERT INTO Inventory VALUES ('25468', 'guava', 478);
```

