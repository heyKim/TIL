# Logstash Basic
## Logstash Config
### config file 구성
logstash config파일은 아래와 같이 크게 세 부분으로 나뉘어져 있다. input-> filter->output 순서로 pipeline이 진행됨.
```
# 수집할 데이터를 설정하는 부분으로 evnets를 생성함
input {
  ...
}
# (optional) 수집한 데이터를 어떻게 거를지 설정하는 부분으로 event를 modify함
filter {
  ...
}
# 전달받은 events를 다른 곳으로 전달함
output {
  ...
}
```

### Event Data와 Field에 Accessing
#### Field References
- name으로 field에 접근하는 기본적인 방법: `[fieldname]`
- nested field에 접근하는 방법: `[top-level field][nested field]`
- (예) `os`필드에 접근하려면 `[ua][os]`로 접근하면 된다.
```
{
  "agent": "Mozilla/5.0 (compatible; MSIE 9.0)",
  "ip": "192.168.24.44",
  "request": "/index.html"
  "response": {
    "status": 200,
    "bytes": 52353
  },
  "ua": {
    "os": "Windows 7"
  }
}
```
#### sprintf format
- sprintf format라고 부르는 포맷을 이용해 필드 안의 값을 가져와 사용할 수도 있다.
- (예) increment 필드에는 "apache"라는 문자열에 `[response][status]`필드의 값이 합쳐진 문자열이 들어간다.
```
output {
  statsd {
    increment => "apache.%{[response][status]}"
  }
}
```
#### `@metadata` field
- `@metadata` field: Logstash 1.5버전 이후로 생긴 필드로 이 필드는 output 구문에서는 빠지는 필드이다.
- 조건문이나 event 필드를 변경할 때 temporay field 개념으로 쓰기 좋은 field이다.
- (예)
```
input { stdin { } }

filter {
  mutate { add_field => { "show" => "This data will be in the output" } }
  mutate { add_field => { "[@metadata][test]" => "Hello" } }
  mutate { add_field => { "[@metadata][no_show]" => "This data will not be in the output" } }
}

output {
  if [@metadata][test] == "Hello" {
    stdout { codec => rubydebug }
  }
}
```
- output이 모두 elasticsearch로 가는데 index가 달라야 하는 경우 `@metadata`필드를 이용하면 유용할 듯하다.
