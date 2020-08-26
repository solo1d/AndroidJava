> **SQLite 与 MySQL 在SQL语句三有很多不同, 尤其是 ; 分号** 

## 安装

```bash
$ sudo apt install -y  sqlite
```



## 进入控制命令行

```bash
$ sqlite3     #会直接进入
```



## 退出

```bash
sqlite>  .quit       #在 sqlite 命令行模式下, 输入 .quit  即可退出
```



## 创建并打开数据库文件

```bash
$ mkdir  ~/data  && cd ~/data     #创建并来到一个存放数据库文件的目录
$ sqlite3   text.db              # 创建 数据库文件 text.db  ,并且进入 sqlite 命令行
```



## 数据库操作

```bash
$ sqlite3   text.db              # 创建 数据库文件 text.db  ,并且进入 sqlite 命令行
sqlite>   .tables               # 查看数据库文件 text.db 内,拥有那些表, 末尾不能有 分号; 

# 新建一个表  user ,  末尾有分号; 
sqlite> CREATE TABLE user(
   ...> id INT PRIMARY KEY NOT NULL,
   ...> name  TEXT NOT NULL,
   ...> age INT NOT NULL
   ...> );


#删除一个表 ,  末尾有分号; 
sqlite>  DROP TABLE user;

#向一个表内插入数据, 用 user为范例,  两种写法 ,  末尾有分号; 
sqlite>  INSERT INTO  user(id,name,age) VALUES(1,'zhang',18);
sqlite>  INSERT INTO user VALUES(2,'wang',19);
 
#查询 ,  末尾有分号; 
sqlite>  SELECT * FROM user where age>=18;
  #输出范例: 
  		1|zhang|18
			2|wang|19	


```

