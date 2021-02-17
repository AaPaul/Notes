## MySql
#### Install
```
sudo apt-get upgrade
sudo apt-get install mysql-server

sudo mysql_secure_installation

#1
VALIDATE PASSWORD PLUGIN can be used to test passwords...
Press y|Y for Yes, any other key for No: N 

#2
Please set the password for root here...
New password: 
Re-enter new password: 

#3
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them...
Remove anonymous users? (Press y|Y for Yes, any other key for No) : N 

#4
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network...
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : N 

#5
By default, MySQL comes with a database named 'test' that
anyone can access...
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : Y

#6
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y
```
You then need to check the status of the MySQL service.
```
systemctl status mysql.service
# if success, you may see
mysql.service - MySQL Community Server
Loaded: loaded
Active:active(running)
```
Configuration of remote connection.
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf #Find bind-address and change it to 0.0.0.0

sudo /etc/init.d/mysql restart 
```
Change the datadir to `/home`
```
# Stop mysql service
sudo service mysql stop

# Note: Please do make a copy of mysql file to prevent data lossing when is on moving.
sudo cp /var/lib/mysql /home/aafc/mysql_copy -r

# Then create a new folder for storing the orginal files.
sudo mkdir /home/aafc/databases
sudo mv /var/lib/mysql /home/aafc/databases/

# Delete log files
sudo rm /home/aafc/databases/mysql/ib_logfile0
sudo rm /home/aafc/databases/mysql/ib_logfile1
```
Modify the configuration of mysql
```

sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
# change the parameter named datadir
datadir = /home/aafc/databases/mysql
```

Modify the configuration of apparmor.
```
sudo vim /etc/apparmor.d/usr.sbin.mysqld
```
You can see these information in the file which shows below and you need to update the path. 
```
# Allow data dir access
#  /var/lib/mysql/ r,
#  /var/lib/mysql/** rwk,
   /home/aafc/databases/mysql/ r,
   /home/aafc/databases/mysql/** rwk,

# quit and restart service
sudo service apparmor reload
```
Modify the service which is to start mysql.
```
sudo vim /usr/share/mysql/mysql-systemd-start

# In this file, we need to change the path from '/var/lib/mysql' to '/home/aafc/databases/mysql'

# quit and restart mysql
sudo service mysql restart

# validate and test if the changes are updateds.
sudo mysql -uroot -p
show variables like '%datadir%'
+---------------+-----------------------+
| Variable_name | Value                 |
+---------------+-----------------------+
| datadir       | /home/database/mysql/ |
+---------------+-----------------------+
1 row in set (0.00 sec)
```

#### Delete
```
sudo apt-get autoremove --purge mysql-server-5.0
sudo apt-get remove mysql-server
sudo apt-get autoremove mysql-server
sudo apt-get remove mysql-common 
```
清理残留文件
```
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```

- references 
https://blog.csdn.net/LiuNian_SiYu/article/details/53048314 \
https://noob.tw/remove-mysql-completely/

#### Website resources
- https://www.thinbug.com
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


#### ERROR 2013: Lost connection to MySQL server during query
There may be three reasons resulting this issue.
- Time out because of two variables which are not big enough. 
```
# set it in /etc/mysql/my.cnf 
net_read_timeout = 120
net_write_timeout = 900

# after saving that, restart mysql server
```

- Setting of `max_allowed_packet` which controls the size of data package which is in importing.
```
# set it in /etc/mysql/my.cnf 
max_allowed_packet      = 500M
``` 

- The table is broken.



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

#### Differences among left join, right join and inner join
- Left Join. Query for all satisfied data in Table 1 and the intersection part in Table 2
```[MySQL]
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;
```
- Right Join. The query will return all satisfied data in Table 2 and the intersection part in Table 1
```[MySQL]
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
```
- Innter Join. the intersection between Table 1 and Table 2
- Reference. https://www.runoob.com/sql/sql-join-left.html

#### Drop tables
If the table contains foreign keys, we can change the setting of mysql to cancel the limitation of connection of foreign key.
```
set_foreign_checks = 1
```

#### Set binlog
In the file named `***/mysqld.conf`, you can set `expire_logs_seconds`.
```
set sql_log_bin= {ON|OFF}
mysql> reset master;  //删除master的binlog，即手动删除所有的binlog日志
mysql> reset slave;  //删除slave的中继日志
mysql> purge master logs before '2019-08-30 17:20:00';  //删除指定日期以前的日志索引中binlog日志文件
mysql> purge master logs to 'binlog.000001';  //删除指定日志文件的日志索引中binlog日志文件
```

#### Error 'Cannot find /tmp/mysql.sock'
```
sudo find / -name mysql.sock
sudo find / -name mysqld.sock

sudo ln -s {path} /tmp/mysql.sock
```

#### Install Apache
```
sudo apt-get update
sudo apt-get install libexpat1
sudo apt-get install apache2 apache2-utils ssl-cert

# Then run the following command to check if the apache ser is responding.
curl http://localhost
```
Some basic commands for managing apache service
```
# start
sudo service apache2 start
sudo apachetl start

# reload website
sudo service apache2 reload
```


If the previous step is OK, we can continue to install wsgi part.
```
sudo apt-get install libapache2-mod-wsgi
sudo systemctl restart apache2
```
Configure apache for WSGI
```
sudo vi /var/www/html/wsgi_test_script.py

# add following part
def application(environ,start_response):
    status = '200 OK'
    html = '<html>\n' \
           '<body>\n' \
           ' Hooray, mod_wsgi is working\n' \
           '</body>\n' \
           '</html>\n'
    response_header = [('Content-type','text/html')]
    start_response(status,response_header)
    return [html]
```
After that, you need to configure the Apache server to serve this file over the HTTP protocol. Now we create a configuration file to serve the `wsgi_test_script.py` script over a sub URL.
```
sudo nano /etc/apache2/conf-available/mod-wsgi.conf

# Add the following information
WSGIScriptAlias /test_wsgi /var/www/html/wsgi_test_script.py

# enabel the configuration and restart 	apache service
sudo a2enconf mod-wsgi
sudo systemctl restart apache2
```
After that, you can open `http://your_ip_address/test_wsgi` successfully, which illstrates that we can allow other user to visit your project.


- reference: https://tecadmin.net/install-apache-mod-wsgi-on-ubuntu-18-04-bionic/


#### Deploy the project
We've already download the required modules on *Installing apache*. Now let's start the deploy the project on apache server.

1. Allow in firewall
```
(base) aafc@aafc-VirtualBox:/etc/apache2/sites-available$ sudo ufw allow in "Apache Full"
Rules updated
Rules updated (v6)

```


2. Copy the project to /var/www/projectname
```
(c3pd) aafc@aafc-VirtualBox:/var/www/c3pd$ sudo cp ~/Desktop/c3pd_sources/c3pd2Ver2/

# create wsgi file
(c3pd) aafc@aafc-VirtualBox:/var/www/c3pd$ sudo nano pedigree.wsgi 

# add the content below
                                          
import sys
from os import path
sys.path.insert(0, path.abspath(path.dirname(__file__)))
from app import app as application

# quit (Ctrl + X) and allocate the authority of this folder to apache
# This is the default setting of Apache and you can find it on /etc/apache2/apache.conf
(base) aafc@aafc-VirtualBox:/etc/apache2/sites-available$ sudo chown -R www-data:www-data /var/www/c3pd/
```

3. Website settings
- create `c3pd.conf` file and add the content. **Note:** In this part, if you use Anaconda to build virtual python environment, you should set the parameter `python-path` to corresponde to its path in anaconda directory.
In Apache 2.4,
```
Order allow, deny
Allow from all

Change to 

Require all granted
```
```
<VirtualHost *:80>
        WSGIDaemonProcess c3pd user=www-data group=www-data home=/var/www/c3pd/ python-path=/var/www/flaskapp:/home/aafc/Tools/anaconda3/envs/c3pd/lib/python2.7/site-packages
        WSGIProcessGroup c3pd
        WSGIScriptAlias / /var/www/c3pd/pedigree.wsgi
        <Directory /var/www/c3pd>
        Require all granted
        </Directory>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
~                  
```
- enable the website
```
sudo a2ensite c3pd.conf

sudo systemctl reload apache2
```


**The potiential errors.**
If the website cannot run normally, you can check the error log to figure out which part is in troubles. 
防火墙是有可能造成网页无法访问的原因之一(500 error), 也有可能是apache没有权限(Default user: www-data, pwd: www-data).
Using `mysql --print-defaults` can output the default configuration when we run mysql service. These settings can be modified on `/etc/mysql/my.cnf`.

```
cat /var/log/apache2/error.log
```

If you meet the problem as follows, 
```
Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```
the first way you should try is to restart mysql as the possible reason is that the mysql service is disconnected and if it doesn't work, you can try to use these commands to solve it (Based on the my laptop).
```
# Check whether the firewall stop the apache server

# Check if the user get the enough authority of the project folder
sudo chown -R www-data /var/www/c3pd

# You should find the path of the right socket you use
mysql --print-defaults
mysql would have been started with the following arguments:
--port=3306 --socket=mysqld.sock

# Then you can copy the address
sudo find / -name mysqld.sock
/var/run/mysqld/mysqld.sock

# Create the soft link between it and /tmp/mysql.sock and restart mysql
sudo ln -s /var/run/mysqld/mysqld.sock /tmp/mysql.sock
sudo systemctl restart mysql
```




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
- If you want to add a new page in a group (blueprint), you should import this file in `__init__.py` as the initialization.
- reference:https://www.zhihu.com/question/31748237

#### Popen
is a kind of function to start another new application or thread in a thread in `Subprocess`

### HTML
#### Form tag
`<form>` is the tag to limit the action of jumping to other file.

#### Difference between 'Submit' and common 'button'
'Submit' button just submit the data in a form to web server, while the 'button' not only can fulfill that but also provides other functions.

#### url_for
quickly find the corresponding url.

## Jinja2
#### contional statement
```
{% for item in {{item_list}} %}

{%endfor}

{% if xxx %}
{% endif %}
```

##### Variables of a for-loop block

|  Variables   | Description  |
|  :----:  | :----:  |
| **loop.index**  | The current iteration of the loop |
| **loop.revindex** | The number of iterations from the end of the loop (1 indexed)|
| **loop.first** | True if first iteration |
| **loop.last**| True if last iteration |
| **loop.length** | The number of items in the sequence |
| **loop.cycle**| To cycle between a list of sequence |
|**loop.depth**| Indicates how deep in deep in a recusive loop the rendering currently is. Starts at level 1|

https://blog.csdn.net/kylinxjd/article/details/94563427  \
https://segmentfault.com/q/1010000000690359/a-1020000000690397

## Apache

#### Common commands
1. Start Apache 2 Server /启动apache服务
```
# /etc/init.d/apache2 start
or
$ sudo /etc/init.d/apache2 start

sudo apachectl start 
```

2. Restart Apache 2 Server /重启apache服务
```
# /etc/init.d/apache2 restart
or
$ sudo /etc/init.d/apache2 restart
```

3. Stop Apache 2 Server /停止apache服务
```
# /etc/init.d/apache2 stop
or
$ sudo /etc/init.d/apache2 stop

sudo apachectl stop 
```

4. Check the version of the apache
```
sudo apachectl -v
```

5. Deploy Flask app on apache (In Mysql part)

6. Check and modify the user group of running Apache
```
# Check the environment
master@ubuntu:~$ apache2 -v
Server version: Apache/2.4.18 (Ubuntu)
Server built:   2019-10-08T13:31:25
master@ubuntu:~$ lsb_release -a
No LSB modules are available.
Distributor ID:    Ubuntu
Description:    Ubuntu 16.04.6 LTS
Release:    16.04
Codename:    xenial
```
In Ubuntu OS, If we use `apt` or `apt-get` to install Apache, we can see the `envvars` file in `/etc/apache2/` which includes the detail of the user group managing Apache. (`Cat /etc/apache2/envvars`).

Authority allocation
```
master@ubuntu:/var/www/html$ sudo chown www-data:www-data -R [your-file]
```
- reference: https://zhaokaifeng.com/?p=3271


#### Multiple project deploy


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

#### Pagination on server side
##### Front-end
```[JavaScript]
$('table-name').DataTable({
        "serverSide": true,
        "ajax": {
                url: "/url",
                type: "POST"
        },
        "bFilter": true,
        "bSort": true
});

```
##### Rear-end
using `concat` in mysql to implement blurry search.
```
def function('/url', methods=['POST', 'GET']):

if request.method = 'POST':
        pagesize = int(request.form.get('length')) # The number size of displaying the table
        start = int(request.form.get('start')) 
        search = request.form.get('search[value]') # value in search bar
        order_column_num = int(request.form.get('order[0][column]'))
        order_dir = request.form.get('order[0][dir]')
        column_dict = {
            0: marker.marker_name,
            1: marker.marker_name_alt,
            2: marker.marker_type,
            3: marker.org_marker_chr,
            4: marker.org_marker_ctg,
            5: marker.mapped_marker_chr,
            6: marker.mapped_marker_pos
        }
        order_column = column_dict[order_column_num]
        if order_dir == 'desc':
            cond = order_column.desc()
        else:
            cond = order_column.asc()
        if app_name=='FlaxDB':
            species_f = 1
            markers_list, message_num = get_markers_info(species_f, cond, start, pagesize, search)
        elif app_name=='4DWheat':
            species_f = 4
            markers_list, message_num = get_markers_info(species_f, cond, start, pagesize, search)
        else:
            markers_list, message_num = get_markers_info_CGBP(cond, start, pagesize, search)
        data = []
        for x, y in enumerate(markers_list):
            m_name = '<a href="/QTL/marker/%s">%s</a>' % (y.id, y.marker_name)
            row = [m_name, y.alt_name, y.marker_type, y.org_marker_chr, y.org_marker_ctg, y.mapped_marker_chr, y.mapped_marker_pos]
            data.append(row)
        messagedata = {
            "recordsTotal": message_num,
            "recordsFiltered": message_num,
            "data": data
        }
        return json.dumps(messagedata)

def get_markers_info(species_f,cond, start, pagesize, search=None):
    if search:
        markers_list = session.query(marker.id, marker.marker_name, marker.marker_name_alt.label('alt_name'),
                                     marker.marker_type,
                                     marker.org_marker_chr,
                                     marker.org_marker_ctg, marker.mapped_marker_chr,
                                     marker.mapped_marker_pos).filter(and_(
            marker.species_f == species_f,
            func.concat(marker.marker_name, ',', marker.id, ',', marker.marker_name_alt, ',',
                        marker.marker_type, ',',
                        marker.org_marker_chr, ',',
                        marker.org_marker_ctg, ',', marker.mapped_marker_chr, ',',
                        marker.mapped_marker_pos).like('%' + search + '%'))).order_by(cond).offset(start).limit(
            pagesize).all()
        message_num = session.query(func.count(marker.id)).filter(and_(
            marker.species_f == species_f,
            func.concat(marker.marker_name, ',', marker.id, ',', marker.marker_name_alt, ',',
                        marker.marker_type, ',',
                        marker.org_marker_chr, ',',
                        marker.org_marker_ctg, ',', marker.mapped_marker_chr, ',',
                        marker.mapped_marker_pos).like('%' + search + '%'))).all()[0]
    else:
        markers_list = session.query(marker.id, marker.marker_name, marker.marker_name_alt.label('alt_name'),
                                     marker.marker_type,
                                     marker.org_marker_chr,
                                     marker.org_marker_ctg, marker.mapped_marker_chr, marker.mapped_marker_pos).filter(
            marker.species_f == species_f).order_by(cond).offset(start).limit(pagesize).all()
        message_num = session.query(func.count(marker.id)).filter(marker.species_f == species_f).all()[0]

    return markers_list, message_num

def get_markers_info_CGBP(cond, start, pagesize, search=None):
    if search:
        markers_list = session.query(marker.id, marker.marker_name, marker.marker_name_alt.label('alt_name'),
                                     marker.marker_type,
                                     marker.org_marker_chr,
                                     marker.org_marker_ctg, marker.mapped_marker_chr,
                                     marker.mapped_marker_pos).filter(
            func.concat(marker.marker_name, ',', marker.id, ',', marker.marker_name_alt, ',',
                        marker.marker_type, ',',
                        marker.org_marker_chr, ',',
                        marker.org_marker_ctg, ',', marker.mapped_marker_chr, ',',
                        marker.mapped_marker_pos).like('%' + search + '%')).order_by(cond).offset(start).limit(
            pagesize).all()
        message_num = session.query(func.count(marker.id)).filter(
            func.concat(marker.marker_name, ',', marker.id, ',', marker.marker_name_alt, ',',
                        marker.marker_type, ',',
                        marker.org_marker_chr, ',',
                        marker.org_marker_ctg, ',', marker.mapped_marker_chr, ',',
                        marker.mapped_marker_pos).like('%' + search + '%')).all()[0]
    else:
        markers_list = session.query(marker.id, marker.marker_name, marker.marker_name_alt.label('alt_name'),
                                     marker.marker_type, marker.org_marker_chr,
                                     marker.org_marker_ctg, marker.mapped_marker_chr,
                                     marker.mapped_marker_pos).order_by(cond).offset(start).limit(pagesize).all()
        message_num = session.query(func.count(marker.id)).all()[0]

    return markers_list, message_num
```

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

#### location.href()
Jumping within pages

#### Using 'POST' method to implement downloading function

## Linux (Ubuntu) 
#### Installation
Mainly for manul partition.
```
/ -> root content. 10-20 GB. ext4 format
/boot -> bootloader. usually 200MB-400MB. ext4 format
/swap -> swap area. same as the size of the memory. swap area format
/home -> store personal files. all last space can be allocated to it. ext4.
```

#### ufw firewall settings
```
# Installation
sudo apt-get isntall ufw

# Check status
sudo ufw status

# Start 
sudo ufw enable
sudo ufw default deny (Allow the host machine access external website and close all external access to this machine)

# Stop
sudo ufw disable

# Turn on or turn off
sudo ufw allow/deny [service or port]
```

#### Soft link
Check link
```
# check the detail in /tmp folder.
ls -l /tmp
```

Build soft link
```
# To solve the error that Cannot find connect mysql through `/tmp/mysql.sock,(2)`
sudo ln -s /var/run/mysqld/mysqld.sock /tmp/mysql.sock 
```

#### the absolute path and relative path
In Ubuntu, it's better to use absolute path in a project to prevent the error that the system cannot find the module which imported with the relative path.

For example:
```
# In test.py which is included in the app folder and there is an another file named main. In that case, there may be no error as they are both in the same directory.
from app.main import main_test
import main 

# Same test.py file and there is a file named outside.py which is in the same directory with app folder.
# In that case, there may be an error because of the missing of the module named main which is imported by the relative path
from app.main import main_test # work
import main # does not work
```

## DataFrame
#### DataFrame to json and transfer the format to dict instead of unicode

1. `eval(df.to_json())`. `name 'null' is not defined` this issue cannot be resolved.

2. `json.loads(df.to_json())`

```
# One
json_1 = eval(df.to_json(orient=xxx)) # cannot support the data with None value

 # {‘split’,’records’,’index’,’columns’,’values’}

 df = pd.DataFrame([['a', 'b'], ['c', 'd']],
                   index=['row 1', 'row 2'],
                   columns=['col 1', 'col 2'])
###########
split
###########
df.to_json(orient='split')
>'{"columns":["col 1","col 2"],
  "index":["row 1","row 2"],
  "data":[["a","b"],["c","d"]]}'
###########
index
###########
df.to_json(orient='index')
>'{"row 1":{"col 1":"a","col 2":"b"},"row 2":{"col 1":"c","col 2":"d"}}'

###########
records
###########
df.to_json(orient='index')
>'[{"col 1":"a","col 2":"b"},{"col 1":"c","col 2":"d"}]'

###########
table
###########
df.to_json(orient='table')
>'{"schema": {"fields": [{"name": "index", "type": "string"},
                        {"name": "col 1", "type": "string"},
                        {"name": "col 2", "type": "string"}],
             "primaryKey": "index",
             "pandas_version": "0.20.0"},
  "data": [{"index": "row 1", "col 1": "a", "col 2": "b"},
           {"index": "row 2", "col 1": "c", "col 2": "d"}]}'



# Two
json_2 = json.loads(df.to_json()) # can support None values.
```

Ref: 
- https://blog.csdn.net/huanbia/article/details/72674832?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight
- https://blog.csdn.net/ANXIN997483092/article/details/79223410
- https://www.cnblogs.com/lowmanisbusy/p/9135917.html

