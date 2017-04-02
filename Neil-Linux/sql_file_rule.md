
# sql 文件的书写规范

## 注释
```
范例：
-- delete existing data
delete from track;
delete from cd;
delete from artist;

或者
#artist表
#   包含id列 和 艺术家name列
create table artist (
    id integer auto_increment not null primary key,
    name varchar(100) not null    
);    
```
