# Docker LAMP + elastic search


## run 

```
docker-compose up -d 
``` 


## settings 

### database
```
host : mysql
port : 3307
``` 
e.g : ```php  define('DB_HOST', 'mysql'); ```


### elastic search 

```
host : elastic
port : 9200
```

e.g : ``` http://elastic:9200/ ``` 

## Wordpress elasticpress 


Autosuggest ? : use this enpoint : ``` http://localhost:9200/_search ``` that will work for POST


## Issues ? 

``` ps not a command ```


``` 
RUN apt-get update && apt-get install -y procps

```

Extend map count : 

``` 

MAC : 

cd ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/
echo "vm.max_map_count=262144" >> sysctl.con

``` 