# ELK_Assignement

## Steps to Stream data from SQL to Elasticsearch

Step 1: pull following docker images from repository
        
         sql_server: docker pull mcr.microsoft.com/mssql/server:2019-latest
         elasticsearch: docker pull docker.elastic.co/elasticsearch/elasticsearch:7.10.1
         kibana: docker pull docker.elastic.co/kibana/kibana:7.10.1
         logstash: docker pull docker.elastic.co/logstash/logstash:7.10.1

Step 2: login and create database then restore data from .bak file using below command
        
        sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U user_name -P 'password' -Q 'RESTORE FILELISTONLY FROM DISK = "/home/ram/Downloads/AdventureWorksLT2019.bak"'

Step 3: Run Elasticsearch, Kibana docker container using below commands
   
        docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.10.1
        check elsaticsearch status: curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"

        docker run --link es_container_id:elasticsearch -p 5601:5601 docker.elastic.co/kibana/kibana:7.10.1
        
Step 4: Copy "sql.conf" file into /etc/logstash/conf.d/ folder inside container, update sql.conf file with proper details such as db name, container name, user, password
Step 5: Run logstash using below command

        docker run --rm -it -v ~/pipeline/:/usr/share/logstash/pipeline/ docker.elastic.co/logstash/logstash:7.10.1


Note: you may need to copy mssql-jdbc-7.4.1.jre8.jar file into /usr/share/logstash/logstash-core/lib/jars/ 




       
