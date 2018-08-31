# ELK-learning
全文检索ElasticSearch, Logstash, Kibana学习

## ElasticSearch
基于Lucene实现的全文检索引擎

## Logstash
支持多种数据输入输出的数据抽取工具

1. Kafka输入，ES输出
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

2. ES输入，ES输出（复制索引）
``` Ruby

input {
  elasticsearch {
      hosts => ["nbot48.dg.163.org:9200", "nbot49.dg.163.org:9200","nbot50.dg.163.org:9200"]
      user => "elastic"
      password => "changeme"
      index => "kaola_xuanpin-jd_all2-2018-08-08"
      query => '{ "query": { "match_all": { } } }'
      docinfo => true
    }
}

filter {

}

output {
  elasticsearch {
    hosts => ["nbot48.dg.163.org:9200", "nbot49.dg.163.org:9200","nbot50.dg.163.org:9200"]
    user => "elastic"
    password => "changeme"
    index => "kaola_xuanpin-jd-2018-08-08"
    document_type => "%{[@metadata][_type]}"
    # 复制document id
    document_id => "%{[@metadata][_id]}"
  }

# stdout { codec => rubydebug }
}
```

## Kibana
ES数据报表工具
