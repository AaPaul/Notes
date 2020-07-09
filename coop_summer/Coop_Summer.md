## MySql
#### 1. Authentication plugin 'caching_sha2_password' cannot be loaded.
This is because the default setting of password is 'caching_sha2_password'. 
```
SELECT user,host,plugin from mysql.user where user='root';
```

Solution:
```
ALTER USER 'username'@'ip_address' IDENTIFIED WITH mysql_native_password BY 'password'; 
flush privileges
```

#### Create user
```
# Create the user named addProgram
Create USER 'addProgram'@'localhost' IDENTIFIED BY 'c1234567';

# NOTE: need to check whether the host is ‘localhost’. If NOT, update it.
update user set host='localhost' where user='addProgram’from mysql.user;

# Create the DB named c3pd2TestingAddProgram
CREATE DATABASE c3pd2TestingAddProgram;

# Assign permissionsGRANT ALL PRIVILEGES ON c3pd2TestingAddProgram.* to 'addProgram'@'localhost';
FLUSH PRIVILEGES;

# UPdate password
USE mysql;
ALTER USER 'addProgram'@'localhost' IDENTIFIED WITH mysql_native_password by 'mettalica’;
FLUSH PRIVILEGES
```


#### ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
Solution: 
```
# Show the original strategy of password
SHOW VARIABLES LIKE 'validate_password%'; 

set global validate_password.policy=LOW;
```

#### DBURI
```
mysql标示位表明是哪个数据库
        xiaorui:123 表明的是账号和密码
                @localhost 表明的是数据库HOST地址
                          /xiaorui_master 表明的是数据库名字
                                            ?k=v&k=v 这堆传参是作为扩展字段使用的
```

#### Get the size of databases
```
USE information_schema;
SELECT TABLE_SCHEMA, SUM(DATA_LENGTH)/1024 FROM TABLES GROUP BY TABLE_SCHEMA;
```
If we wanna use KB as the unit of measurement, we over 1024 again. So does the GB

#### Update the password
```
USE mysql;
ALTER USER 'addProgram'@'localhost' IDENTIFIED WITH mysql_native_password by 'mettalica’;
FLUSH PRIVILEGES;
```

#### Assign permissions of one database to a user
```
GRANT ALL PRIVILEGES ON c3pd2TestingAddProgram.* to 'addProgram'@'localhost';
FLUSH PRIVILEGES;
```

#### Show the encoding standard
show variable like "char%";

#### Show port
'''show global variable like 'port' '''

#### Export/Import database
```[sql]
create database db1; (the name of aimed database)
use db1;
source <path/db1.sql>

# method 2
mysql -u<user> -p db1 < db1.sql

```

#### Distinct
1. eliminate the repeated results when you use ```SELECT DISTINCT```

2. IF there are values which are NULL, the result with DISTINCT would include only one NULL value.

3. In using with multiple columns, the results would be the non-duplicate combination.

Reference:https://www.yiibai.com/mysql/distinct.html

#### Limit
1. following one parameter means that the number of the results will be limited with this parameter
2. following two parameters (i, j) means that the system would jump to the ith row and search the later j number of the results.

## Flask
#### flask 中的before_request 与 after_request

https://www.jianshu.com/p/0e0eb9a486de
https://blog.51cto.com/xficc/1676591

#### app.config
app.config is the sub-child of dictionary which can be used to set the information of configure no matter which is the default or defined by yourself.

https://blog.csdn.net/tanyjin/article/details/78640987

#### GET /static/style/bootstrap.min.css.map HTTP/1.1" 404 -
delete /*# sourceMappingURL=bootstrap.min.css.map */ from bootstrap.min.css, and then the issue is gone.

#### Blueprint

reference:https://www.zhihu.com/question/31748237

#### Popen
is a kind of function to start another new application or thread in a thread in `Subprocess`

### HTML
#### Form tag
`<form>` is the tag to limit the action of jumping to other file.

#### Difference between 'Submit' and common 'button'
'Submit' button just submit the data in a form to web server, while the 'button' not only can fulfill that but also provides other functions.

#### url_for
quickly find the corresponding url.

## Apache

#### Common commands
一、Start Apache 2 Server /启动apache服务
```
# /etc/init.d/apache2 start
or
$ sudo /etc/init.d/apache2 start

二、 Restart Apache 2 Server /重启apache服务

# /etc/init.d/apache2 restart
or
$ sudo /etc/init.d/apache2 restart

三、Stop Apache 2 Server /停止apache服务

# /etc/init.d/apache2 stop
or
$ sudo /etc/init.d/apache2 stop
```


## SQLAlchemy
####        db.session.query(User).filter_by(name='Tom').all()

It is to find the object whose name is 'Tom' in table named 'User'

#### sqlalchemy.ext.automap
It is a function to map the table in the DB to an object. (映射数据库表为一个python可处理对象)

#### There is no right cross in SQLAlchemy 
inner join -> join('tablename', 'condition')
leftjoin -> outerjoin('tablename', 'condition')

## Pip
#### Use cached xxxx (error)
```
pip --no-cache-dir install <packagename>
```

## Client & Server
#### Cookie
It is a kind of data. After the browser visites the server, the server would pass data to the browser.

1. Function
- Recognize the users (identity)
- Mark the history


#### urllib, urllib2 (py2)[1]
1. receive an example of Request and set the headers of URL requests.

2. python can use urllib and cookielib to request an url with the required authority.

3. url can only receive URL and provide *urlencode* to query the string while urllib2 cannot.

##### reference
[1]: https://blog.csdn.net/drdairen/article/details/51149498

#### Show the original sql
```engine = create_engine("your_db_uri", echo=True)```
**Reference**:https://blog.csdn.net/shuaizy2017/article/details/80326868


## Anaconda
#### Export and import environment
```
conda env export > <name>.yaml

conda list -e > requirements.txt

pip freeze > requirements.txt
pip install --no-cache-dir -r requirements.txt



conda env create -f environment.yaml
conda install --yes --file requirements.txt
```


## Redis
#### Basic setting
```
# install
brew install redis

# start & stop
redis-server 
redis-cli shutdown
```

#### rq_dashborad
rq-dashboard is a general purpose, lightweight, Flask-based web front-end to monitor your RQ queues, jobs, and workers in realtime.

## JavaScript
#### $() repersent calling a function

#### \# represents a variable name

#### Option is an option in the drop-down list in HTML form
