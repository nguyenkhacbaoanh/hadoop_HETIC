# Technologie BigData

## Hadoop

### Virtual Machine
Mon ip : 192.168.56.101


Launch hdfs : 
start-dfs.sh
jps
start-yarn.sh

Hadoop interface : 192.168.56.101:50070
Yarn interface : 192.168.56.101:8088

Connect to file system of hdfs :
hdfs dfs -ls /
or
hadoop fs -ls /

Move file :
```terminal
hdfs dfs -put dat_svi_data.csv /user/hadoop/data/
```
Activate Hive :
hive
Hive commands :
```SQL
SHOW DATABASES;
SHOW DATABASE test;
SHOW TABLES;

SELECT FROM_UNIXTIME(UNIX_TIMESTAMP5());

Create data structure scheme:
CREATE TABLE svi_data (
    calldate string,
    calltime string,
    callid bigint,
    calltype string,
    calltype2 string,
    phonenumber string,
    userid bigint)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\;';

LOAD DATA INPATH '/user/hadoop/data/dat_svi_data.csv' INTO TABLE svi_data;

SELECT * FROM svi_data LIMIT 10;

SELECT COUNT(*) FROM svi_data;

create external table svi_data_ext (
        call_date string,
        call_time string,
        vdn int,
        offer string,
        call_queue string,
        `user` string,
        call_id int
    )
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '\;'
	LOCATION '/user/hadoop/data';


CREATE TABLE svi_data_pqt (
    calldate string,
    calltime string,
    callid bigint,
    calltype string,
    calltype2 string,
    phonenumber string,
    userid bigint)
STORED AS PARQUET;

INSERT OVERWRITE TABLE svi_data_pqt SELECT * FROM svi_data;

CREATE TABLE svi_data_orc (
    calldate string,
    calltime string,
    callid bigint,
    calltype string,
    calltype2 string,
    phonenumber string,
    userid bigint)
STORED AS ORC;

INSERT OVERWRITE TABLE svi_data_orc SELECT * FROM svi_data;

```

## SPARK
```terminal
spark-sql --master yarn-client
spark-sql --master local[*]
```
interface spark : 192.168.56.101:4040

## Data set
Unpackage tar.gz => tar -zxvf *.tar.gz

## Object data (Json)
- create a folder `json` on hdfs
```terminal
hdfs dfs -mkdir /user/hadoop/json
```
- put all files json on local to cluster hdfs
```terminal
cd public/json
hdfs dfs -put *.json /user/hadoop/json/
```

## Launch Spark version Python: `Pyspark`
Open a terminal and write this
```terminal
$ pyspark
```

```Python
from pyspark import HiveContext 
sc
hctx = HiveContext(sc)
hctx
df = hctx.read.json('/user/hadoop/json')
df.printSchema()
df.write.format('parquet').saveAsTable('datasource.json_google')
```
- instruction of data
![](https://i.imgur.com/o1s6niv.png)

- `LATERAL VIEW EXPLODE(results) MT1` like property `flatten`in `numpy`, generally, it converts all elements in a object(array) to line 
```hive
DESCRIBE json_google;
SELECT status, extracted, result FROM json_google
LATERAL VIEW EXPLODE(results) MT1 as result LIMIT 1;

SELECT status, extracted, result.name FROM json_google
LATERAL VIEW EXPLODE(results) MT1 as result LIMIT 100;

SELECT status, extracted, result.geometry.location FROM json_google
LATERAL VIEW EXPLODE(results) MT1 as result LIMIT 100;

CREATE TABLE googleplace AS SELECT status, extracted, result.name, result.geometry.location FROM json_google LATERAL VIEW EXPLODE(results) MT1 as result;

SELECT ROUND(location.lat,0), ROUND(location.lng,0),
COLLECT_SET(name) as hotels
FROM googleplace
GROUP BY ROUND(location.lat,0), ROUND(location.lng,0);
```

██─▄███▄███▄─██▄──▄██──▄███▄──██──██
██─█████████──▀████▀──██▀─▀██─██──██
██──▀█████▀─────██────██▄─▄██─██──██
██────▀█▀───────██─────▀███▀──▀████▀ 

 / '.        .--.
 |   \     .'-,  |
 \    \___/    \_/
  '._.'   '._.'
    /  . .  \
    |=  Y  =|
    \   ^   /
     `'---'`
     

---

# Chat
**anon1** : Hello world !
**Belette anonyme**: Hello !
**Belette anonyme**: Non c'est pas moi !
**anon1** : Bush Mastah !
**noname**: I dont know who i am :+1:
**Johnny** : Coucou ! :optic2000:
**anon1** : Qui est chaud pour faire un truc jeudi à la place d'autonomie ?

**vic vaporub**: ca marche pour vous ? (MERCI BEAUCOUP CHARLES <3)
**Johnny** : Ouais ça marche, t'es bloqué ?
**vic vaporub**: ouaip a chaque fois que je fais un -put ca merde --'
**noname**: c'est quoi 'un truc' pour jeudi?
**anon1** : Comme tu veux : foot, atelier CV/stage, ciné, mini golf, mini hackathon, challenge kaggle ...
**noname**: je vote `mini hackathon, challenge kaggle`
**PetitPanda**: je vote atelier stage !
*Belette anonyme a ragequit la discussion*
