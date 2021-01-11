# Logstash Filter

## grok
### grok 참고 사이트
- [Logstash Grok Filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-grok.html)
- [Grok Debugger](https://grokdebug.herokuapp.com/)
- [Grok Patterns](https://github.com/logstash-plugins/logstash-patterns-core/blob/master/patterns/grok-patterns)

### grok 필터 설정 시 주의사항(?)
grok필터 설정하고 logstash를 올렸을 때 `'_grokparsefailure'`라는 tag가 로그에 찍혔다. 패턴은 분명 맞는데 왜 자꾸 이런 에러가 나는건지 한참 찾았다.
이건 pattern이 틀린게 아니고 grok pattern의 input을 잘 못 넣는 등 parsing 에러 때문에 나는 문제이다.
#### `_grokparsefailure` 사례
http_poller를 input으로 설정하고 metadata의 url을 grok의 input으로 넣고 싶었다.
```
"http_poller_metadata" => {
             "request" => {
        "method" => "get",
           "url" => "http://192.168.99.100:9200/services/external/1"
    },
    "response_headers" => {
        "transfer-encoding" => "chunked",
             "content-type" => "application/json; charset=UTF-8"
    },
                "code" => 200,
    "response_message" => "OK",
       "times_retried" => 0,
     "runtime_seconds" => 0.005,
                "name" => "service1",
                "host" => "4bf19441e414"
}
```
grok에 input으로 아래와 같이 설정하니 `'_grokparsefailure'`같은 tag가 로그에 찍혔다.  
```
# ERROR
filter {
    grok {
        match => { “%{[http_poller_metadata][request][url]}” => “%{grok_patterns}” }
    }
}
```
아래와 같이 input을 올바르게 넣어주니 잘 동작했다.
```
# SUCCESS
filter {
    grok {
        match => { “[http_poller_metadata][request][url]” => “%{grok_patterns}” }
    }
}
```
### grok 예시
#### 특정 문자열이 있는지
```
grok {
  # 메세지 필드에 test나 testing 스트링이 있다면
  match => { "message" => ["test", "testing"]
}
```
#### `_grokparsefailure` 태그가 있다면 drop
```
grok {
  if "_grokparsefailure" in [tags] {
    drop {}
  }
}
```
#### IIS(Microsoft Internet Information Services) 로그 파싱하기
```
grok {
  match => {"message" => "%{TIMESTAMP_ISO8601:log_timestamp} %{IP:s-ip} %{URIPROTO:cs-method} %{URIPATH:cs-uri-stem} %{NOTSPACE:cs-uri-query} %{NUMBER:s-port} %{NOTSPACE:cs-username} %{IP:c-ip} %{NOTSPACE:csuser-agent} %{NUMBER:sc-status} %{NUMBER:sc-substatus} %{NUMBER:sc-win32-status} %{NUMBER:time-taken}"}
}
```

#### http_poller의 metadata에서 데이터 추출
- 로그형태: `http://10.111.222.3333:8080/search?select=*&from=app.app1&limit=1`
- 요구사항: 로그에서 ip와 from(application id) 추출
```
filter {
  grok {
    match => "[http_poller_metadata][request][url]" => "%{URIPROTO:uri_proto}://%{URIHOST:ip_port}?(?:%{URIPATH}(?:%{URIPARAM:uri_param})?)?"}
  }
  kv {
    source => "uri_param"
    field_split => "&"
    include_keys => ["from"]
  }
}
```
#### Apache Tomcat Log
- 로그 형태: `0:0:0:0:0:0:0:1 - - [25/Dec/2016:18:37:00 +0800] "GET / HTTP/1.1" 200 11418`
```
grok {
  match => { "message" => '%{IP:client_ip} %{USER:ident} %{USER:auth} \[%{HTTPDATE:timestamp}\] "%{WORD:method} %{DATA:api_data} HTTP/%{NUMBER:http_version}\" %{NUMBER:status} %{NUMBER:bytes_sent}' }
}
```
- 주의: `\[%{HTTPDATE:timestamp}\]`와 같이 대괄호 앞에 \ 없으면 안됨

## split
 - 하나의 event를 여러 events로 쪼갤 때 주로 씀.(array field를 받아서 array안의 각각의 element들로 쪼개는 등에 쓰임)

## ruby 
#### row를 column으로 바꾸는 예제
- 필터 전 로그: `2020.01.23 11:59 [10.192.199.199:6379] 0:336 1:103168 2:112298 3:143790 4:239296 ...(중략).. 12:5276`
- 필터 후 로그: 
```
  { "timestamp": "2020.01.23 11:59", "redis_ip": "10.192.199.199", "redis_port": "6379", "keyspace": "0", "key_count":"336"},
  { "timestamp": "2020.01.23 11:59", "redis_ip": "10.192.199.199", "redis_port": "6379", "keyspace": "1", "key_count":"103168"},
  { "timestamp": "2020.01.23 11:59", "redis_ip": "10.192.199.199", "redis_port": "6379", "keyspace": "2", "key_count":"112298"},
  (중략)
  { "timestamp": "2020.01.23 11:59", "redis_ip": "10.192.199.199", "redis_port": "6379", "keyspace": "12", "key_count":"5276"},
```
```
filter {
  ruby {
    init => 'require "json"'
    code => '
      arrayOfEvents = Array.new()
      # message field 값을 줄바꿈 단위로 자름
      source_list = evnet.get("message").split("\n")
      for source_val in source_list
        source_arr = source_val.split(" ")
        for i in 3..source_arr.length-1
          redis_hostport = source_arr[2].gsub("\[","").gsub("\]","").split(":")
          redis_key_arr = source_arr[i].split(":")
          arrayOfEvents.push({
            "timestamp" => source_arr[0]+" "+source_arr[1],
            "redis_ip" => redis_hostport[0],
            "redis_port" => redis_hostport[1],
            "keyspace" => redis_key_arr[0],
            "key_count" => redis_key_arr[1]
          })
        end
      end
      # puts(arrayOfEvents)
      event.set("results", arrayOfEvents)
    '
  }
  split {
    # results 필드의 value를 각각 event로 생성(split an event to multiple events)
    field => "results"
  }
  mutate {
    rename => {
      "[result][timestamp]" => "timestamp"
      "[result][redis_ip]" => "redis_ip"
      "[result][redis_port]" => "redis_port"
      "[result][keyspace]" => "keyspace"
      "[result][key_count]" => "key_count"
    }
    remove_field => ["results"]
  }
}
```
