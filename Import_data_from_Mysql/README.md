[Wiki link](https://en.wikipedia.org/wiki/MySQL)

### Description

MMySQL is an open-source relational database management system (RDBMS). Its name is a combination of "My," the name of co-founder Michael Widenius's daughter, and "SQL," the abbreviation for Structured Query Language. The MySQL development project has made its source code available under the terms of the GNU General Public License, as well as under a variety of proprietary agreements. MySQL was owned and sponsored by a single for-profit firm, the Swedish company MySQL AB, now owned by Oracle Corporation. For proprietary use, several paid editions are available and offer additional functionality. 

### Instalation

Update Debian package information and install mysql-server. By default in Debian GNU/Linux 9, the open source MariaDB version is downloaded.

```
apt-get update && apt-get install mysql-server
```

Check version (10.1.26)

```
mysql --version
mysql  Ver 15.1 Distrib 10.1.26-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```

### Logstash configuration

We import data from MySQL via Logstash as it's the server-side processing pipeline. For this particular reason, we create a specific MySQL config file.

nano /etc/logstash/conf.d/mysql.conf

```
input {
        jdbc {
                jdbc_driver_library => "/home/mariadb-java-client-2.3.0.jar"
                jdbc_driver_class => "org.mariadb.jdbc.Driver"
                jdbc_connection_string => "jdbc:mariadb://localhost:3306/movielens"
                jdbc_user => "root"       # Security is not a concern at this point, for production different policies
                jdbc_password => "root"
                jdbc_validate_connection => true
                statement => "SELECT * FROM movies"
        }
}
output {
        stdout { codec => json_lines }
        elasticsearch {
                "hosts" => "localhost:9200"
                "index" => "movielens-sql"
                "document_type" => "data"
        }
}
```


### Download movie dataset

```
wget http://files.grouplens.org/datasets/movielens/ml-100k.zip
```

### Create movies db structure

```
MariaDB [(none)]> create database movielens;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create table movielens.movies(
    -> movieID INT PRIMARY KEY NOT NULL,
    -> title TEXT,
    -> releaseDATE DATE);
Query OK, 0 rows affected (0.12 sec)
```

### Setup mysql user and permissions

As per logstash config file we need to setup a mysql user/pass. 
As this is not in production we are not concerned about security. For simplicity we setup root user with root password and grant all access

```
MariaDB [(none)]> CREATE USER root IDENTIFIED BY 'root';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> GRANT USAGE ON *.* TO root@localhost IDENTIFIED BY 'root';
Query OK, 0 rows affected (0.00 sec)

```

### Load data into mysql

```
MariaDB [(none)]>  LOAD DATA LOCAL INFILE 'ml-100k/u.item' INTO TABLE movielens.movies FIELDS TERMINATED BY '|' (movieID, title, @var3) set releaseDate = STR_TO_DATE(@var3, '%d-%M-%Y');
Query OK, 1682 rows affected, 1691 warnings (0.16 sec)
Records: 1682  Deleted: 0  Skipped: 0  Warnings: 1691
```


### Mysql import from logstash

```
root@elastic:~#  /usr/share/logstash/bin/logstash -f  /etc/logstash/conf.d/mariadb.conf
WARNING: Could not find logstash.yml which is typically located in $LS_HOME/config or /etc/logstash. You can specify the path using --path.settings. Continuing using the defaults
Could not find log4j2 configuration at path /usr/share/logstash/config/log4j2.properties. Using default config which logs errors to the console
[WARN ] 2018-11-17 06:38:42.291 [LogStash::Runner] multilocal - Ignoring the 'pipelines.yml' file because modules or command line options are specified
[INFO ] 2018-11-17 06:38:42.326 [LogStash::Runner] runner - Starting Logstash {"logstash.version"=>"6.5.0"}

--------------------------------------------------------------------------------------------------------------------------------------

{"title":"Toy Story (1995)","releasedate":"1995-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:38:55.309Z","movieid":1,"@version":"1"}
{"title":"GoldenEye (1995)","releasedate":"1995-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:38:55.340Z","movieid":2,"@version":"1"}
{"title":"Four Rooms (1995)","releasedate":"1995-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:38:55.342Z","movieid":3,"@version":"1"}
{"title":"Get Shorty (1995)","releasedate":"1995-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:38:55.343Z","movieid":4,"@version":"1"}
{"title":"Copycat (1995)","releasedate":"1995-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:38:55.344Z","movieid":5,"@version":"1"}
{"title":"Shanghai Triad (Yao a yao yao dao waipo qiao) (1995)","releasedate":"1995-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:38:55.345Z","movieid":6,"@version":"1"}

--------------------------------------------------------------------------------------------------------------------------------------

{"title":"Mat' i syn (1997)","releasedate":"1998-02-06T05:00:00.000Z","@timestamp":"2018-11-17T11:39:04.056Z","movieid":1678,"@version":"1"}
{"title":"B. Monkey (1998)","releasedate":"1998-02-06T05:00:00.000Z","@timestamp":"2018-11-17T11:39:04.056Z","movieid":1679,"@version":"1"}
{"title":"Sliding Doors (1998)","releasedate":"1998-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:39:04.088Z","movieid":1680,"@version":"1"}
{"title":"You So Crazy (1994)","releasedate":"1994-01-01T05:00:00.000Z","@timestamp":"2018-11-17T11:39:04.089Z","movieid":1681,"@version":"1"}
{"title":"Scream of Stone (Schrei aus Stein) (1991)","releasedate":"1996-03-08T05:00:00.000Z","@timestamp":"2018-11-17T11:39:04.089Z","movieid":1682,"@version":"1"}
[INFO ] 2018-11-17 06:39:05.348 [[main]>worker0] pipeline - Pipeline has terminated {:pipeline_id=>"main", :thread=>"#<Thread:0x251b1093 run>"}
```

### Check import

```
MariaDB [(none)]> select * from movielens.movies limit 5;
+---------+-------------------+-------------+
| movieID | title             | releaseDATE |
+---------+-------------------+-------------+
|       1 | Toy Story (1995)  | 1995-01-01  |
|       2 | GoldenEye (1995)  | 1995-01-01  |
|       3 | Four Rooms (1995) | 1995-01-01  |
|       4 | Get Shorty (1995) | 1995-01-01  |
|       5 | Copycat (1995)    | 1995-01-01  |
+---------+-------------------+-------------+
5 rows in set (0.00 sec)
```