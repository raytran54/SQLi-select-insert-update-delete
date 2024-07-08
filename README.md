# SQLi-select-insert-update-delete
- Create a db for the lab:
```
Create database newdb;
use newdb
CREATE TABLE users
(
id int(3) NOT NULL AUTO_INCREMENT,
username varchar(20) NOT NULL,
password varchar(20) NOT NULL,
PRIMARY KEY (id)
);
```
- Now let’s insert some sample data into our database. The syntax would be

```
insert into users (id, username, password) values (1, ‘Jane’, ‘Eyre’);
```
- Using name_const()
- 
Syntax:
```
insert into users (id, username, password) values (1,”*(select*from(select(name_const(version(),1)),name_const(version(),1))a)* ”, ‘Eyre’);
```
Danh
