# Docker LAMP + elastic search


## run 

``` bash

docker-compose up -d 

``` 


## settings 

### database
```
host : mysql
port : 3307
``` 
e.g : ```  define('DB_HOST', 'mysql'); ```


### elastic search 

```
host : elastic
port : 9200
```

e.g : ``` http://elastic:9200/ ``` 

## Wordpress elasticpress 


Autosuggest ? : use this enpdoint : ``` http://localhost:9200/_search ``` that will work for POST


## Run containers on linux 

You must create config file before typing ```docker-compose up -d``` otherwise docker will create folders instead of files. 

In the docker-compose.yml Path must be changed from relative to absolute 


## Issues ? 

```  ps: command not found ```


``` bash
apt-get update && apt-get install -y procps

```

Extend map count : 

For mac :

``` bash

cd ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/
echo "vm.max_map_count=262144" >> sysctl.con

``` 
