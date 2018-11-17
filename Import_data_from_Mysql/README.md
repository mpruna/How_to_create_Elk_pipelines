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
