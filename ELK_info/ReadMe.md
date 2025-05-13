# Elasticsearch, logStash, and Kibana

**Elasticsearch**
Uses NoSQL database
full-text search, filtering, and aggregation
used for log management, log aggregation, and log analysis in distributed systems and containerized environments.

**logStash**
It process through the **Logstash pipeline** script
It supports a wide range of input plugins to collect data from sources like log files, syslog, metrics, databases, and message queues. Logstash allows you to parse, filter, and modify data using filter plugins before sending it to Elasticsearch for storage and analysis.

**Kibana**
data visualization and analytics platform that works seamlessly with Elasticsearch to visualize and explore log data.

**FLOW**

Appilication --->Log_file ----> logStash(data processing)---->elasticsearch(storage)---->kibana(Visualize)


**Installation on ubuntu and configuring each individually....**

1) elastic search

wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.12.0-linux-x86_64.tar.gz
tar -xvzf <filename>
mv <file-name> to elasticSearch
bin/elasticSearch

or

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
sudo apt update
sudo apt install elasticsearch
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch

2) Logstash

https://artifacts.elastic.co/downloads/logstash/logstash-8.12.0-linux-x86_64.tar.gz
tar -xvzf <filename>
mv <file-name> to logstash
bin/logstash
or

wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg
sudo apt-get install apt-transport-https
echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-8.x.list
sudo apt-get update && sudo apt-get install logstash

```
bin/logstash -f /path/to/logstash.conf

```

2) Kibana

https://artifacts.elastic.co/downloads/kibana/kibana-8.12.0-linux-x86_64.tar.gz
tar -xvzf <filename>
mv <file-name> to kibana
bin/kibana

or

sudo sh -c 'echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" > /etc/apt/sources.list.d/elastic-7.x.list'
sudo apt update
sudo apt install kibana
sudo systemctl start kibana
sudo systemctl enable kibana
Kibana runs on the **5601** port.

*go to kibana.yml from config dir
uncomment the below line
elasticsearch.hosts:["http://localshost:9200"]  =>mention server address of elasticSearch and its exposed ports




**Steps::**

git clone https://github.com/Java-Techie-jt/elk-stack-logging-example.git
go to the src/main/resources/appilication.yml 
Add the below line
logging:

    file:   <path-of-the-mounted-volume or custom log path>
Crete the logstash.conf file and copy in /bin of logstash
$ \bin\logstash -f logstash.conf
generate the logs..by hitting the getuser api
we will see the generated logs from logstash console also
verify the indices from elasticsearch server where logstash cretes indices like logstash-YYYY.MM.dd
<Elastci-search-instance-ip>/_cat/indices/
<Elastci-search-instance-ip>/<logstash-YYYY.MM.dd>
Kibana-server-->management-->index-patterns-->create index pattern -->give logstash-*
select resulted logstash index pattern -->click next step -->create index pattern
Click on the discover for the logs.



**IMP NOTE**

Analysis(elasticSearch or MongoDB)-->Archiving(S3)-->Monitoring(Cloudwatch,nagios)-->Alerting(SNS,EMAIL).


**For kubernetes pods logging**

create the new logging namespace and delpoy the ELK stack pods..
kubectl apply -f elasticsearch.yaml -n logging
kubectl apply -f logstash.yaml -n logging
kubectl apply -f kibana.yaml -n logging

Deploying the ELK stack components within a dedicated namespace like logging provides better organization, resource isolation, and management of logging-related resources within the Kubernetes cluster. It also allows for easier monitoring and troubleshooting of the logging infrastructure.

