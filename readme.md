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

### securing the instance 

You can install X-Pack for Elasticsearch directly by running:

```
cd /usr/share/elasticsearch
sudo bin/elasticsearch-plugin install x-pack
```

To continue the installation, enter y when prompted. This command will install the X-Pack plugin to your system. When installed, X-Pack enables authentication for Elasticsearch. The default username is elastic and password is changeme. You can check if authentication is enabled by running the same command you ran to check if Elasticsearch is working.

```
curl -XGET 'localhost:9200/?pretty'
``` 

Now the output will say that authentication has failed.
```
user@vultr:~# curl -XGET 'localhost:9200/?pretty'
{
  "error" : {
    "root_cause" : [
      {
        "type" : "security_exception",
        "reason" : "missing authentication token for REST request [/?pretty]",
        "header" : {
          "WWW-Authenticate" : "Basic realm=\"security\" charset=\"UTF-8\""
        }
      }
    ],
    "type" : "security_exception",
    "reason" : "missing authentication token for REST request [/?pretty]",
    "header" : {
      "WWW-Authenticate" : "Basic realm=\"security\" charset=\"UTF-8\""
    }
  },
  "status" : 401
}
```

Change the default password changeme by running the following command.

```
curl -XPUT -u elastic:changeme 'localhost:9200/_xpack/security/user/elastic/_password?pretty' -H 'Content-Type: application/json' -d'
{
  "password": "NewElasticPassword"
}
'
``` 

Replace NewPassword with the actual password you want to use. You can check if the new password is set and Elasticsearch is working by running the following command.

```
curl -XGET -u elastic:NewElasticPassword 'localhost:9200/?pretty'    
```

Information from [here](https://www.vultr.com/docs/how-to-install-and-configure-elastic-stack-elasticsearch-logstash-and-kibana-on-ubuntu-17-04)


To make auth request with WordPress

```php 

$response = wp_remote_post( 'http://elastic:9200/_search', array(

		'body' => '	
			{
				"query": {
					"query_string": {
						"query": " '.$params['query'].' AND (post_type:attachment)"
					}
				}
			}

		',
		'headers' => array( 
			'content-type' => 'application/json; charset=UTF-8',
			'Authorization' => 'Basic ' . base64_encode( 'LOGIN' . ':' . 'PASSWORD')
		),
	
	));

if ( is_wp_error( $response ) ) {
   $error_message = $response->get_error_message();
   echo "Something went wrong: $error_message";
} else {

	echo $response['body'];
	die();	
}

``` 


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
