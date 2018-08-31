# ELK-learning
全文检索ElasticSearch, Logstash, Kibana学习

## ElasticSearch
基于Lucene实现的全文检索引擎

## Logstash
支持多种数据输入输出的数据抽取工具

* 例1，Kafka输入，输出到ES
``` Ruby
input {
  kafka {
    type => "videodata"
    bootstrap_servers => "192.168.0.10:9092"
    topics => ["videodata"]
    client_id => "logstash-videodata"
    codec => "json"
  }
}

filter {
  if [type] == "monitor" {
  }
}

output {
  if [type] == "videodata" {
    elasticsearch {
      hosts => ["192.168.0.1:9200", "192.168.0.2:9200","192.168.0.3:9200"]
      user => "elastic"
      password => "changeme"
      index => "videodata-%{+YYYY}"
    }
  }

  # stdout { codec => rubydebug }
}

```

## Kibana
ES数据报表工具
