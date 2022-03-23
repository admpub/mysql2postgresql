mysql2postgresql
================

Converter mysql schema and data to postgresql

Usage

1. Create dump in xml format using command: `mysqldump --xml -u USER_NAME DB_NAME > DUMP_FILE_NAME`
2. Run converter using command: `php convertor.php -i DUMP_FILE_NAME -o PSQL_FILE_NAME`

convert.sh:

```sh
db_host="localhost"
db_username="root"
db_password="root"
db_name="nging"
db_charset="utf8"
db_port=3306

mysqldump --xml -d $db_name -h$db_host -P$db_port -u$db_username -p$db_password --default-character-set=$db_charset --single-transaction --set-gtid-purged=OFF | sed 's/ AUTO_INCREMENT=[0-9]*\s*//g' > ./install.xml

#echo 按任意键继续
#read -n 1
php convertor.php -i ./install.xml -o ./install.psql
```

导入到PostgreSQL:

```sh
psql -d db_name -U db_username -f ./install.psql
```


Additional options

* `-b50` - set batch count (used on insert data). By default batch count = 200
* `-n` - non export structure


Restriction

This converter does not support foreign keys, because mysql does not return foreign key in xml dump 
Also You must have installed locally php postgresql extension
