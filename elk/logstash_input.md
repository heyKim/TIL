# Lostash Input

## Input Plugins(써본거 위주로..)

### beats
Elastic Beats에서 events를 받는 plugin

### elasticsearch
elasticsearch cluster로 query를 날리고 받은 results를 input으로 사용하는 plugin

### file
파일로부터 stream events 받아 input으로 사용

### http_poller
HTTP API 를 call하고 그 결과를 events로 decode하여 input으로 사용
```
input {
  http_poller {
    # urls 필드는 required
    urls => {
      test1 => "http://localhost:9200"
      test2 => {
        # Supports all options supported by ruby's Manticore HTTP client
        method => get
        url => "http://localhost:9200/_cluster/health"
        headers => {
          Accept => "application/json"
        }
        auth => {
          user => "AzureDiamond"
          password => "hunter2"
        }
      }
    }
    request_timeout => 60
    interval => 60
    # Supports "cron", "every", "at" and "in" schedules by rufus scheduler
    schedule => { cron => "* * * * * UTC"}
    codec => "json"
    # A hash of request metadata info (timing, response headers, etc.) will be sent here
    metadata_target => "http_poller_metadata"
  }
}
output {
  # output을 로그찍어 확인
  stdout {
    codec => rubydebug
  }
}
```

### jdbc
- 이 플러그인을 이용해 db 데이터를 읽어온다. 
- 쿼리 결과 row 1개가 event 1개로 인식된다.
- (예)
```
input {
  jdbc  {
    jdbc_connection_string => "jdbc:mysql://13.125.21.52:3306/fc"
    jdbc_validate_connection => true
    jdbc_user => "fc"
    jdbc_password => "fc"
    jdbc_driver_library => "/home/ec2-user/fc/logstash-5.6.4/driver/mysql-connector-java-5.1.36/mysql-connector-java-5.1.36-bin.jar"
    jdbc_driver_class => "com.mysql.jdbc.Driver"
    statement => "SELECT * FROM sample"
  }
}

output {
 stdout {
   codec => rubydebug
  }
}
```
- 설정
  - `jdbc_connection_string`: 사용하는 db종류, db host, port, db명을 설정
  - `jdbc_driver_class`: 사용하는 jdbc driver 설정
    - oracle: `"oracle.jdbc.driver.OracleDriver"`
    - mysql: `"com.mysql.jdbc.Driver"`
  - `jdbc_driver_library`: 사용하는 jdbc driver 위치 설정
  - `jdbc_user`, `jdbc_password`: DB ID/PW
  - `jdbc_validate_connection`: jdbc로 제대로 연결되었는지 확인(default는 false)
  - `schedule`: staetement (SQL query)를 언제 또는 얼마나 주기적으로 실행할지 설정
  - `statement` : db query
  - `use_column_value`: false시 데이터 트레킹시 사용할 컬럼을 타임스탬프 사용, true시 숫자사용(?)
  - `tracking_column`: 트레킹에 사용할 컬럼
  - `parameters`: statement에서 parameter 값을 받아서 사용 가능
  ```
  input {
      jdbc {        
          jdbc_driver_library => "/etc/logstash/mysql-connector-java-8.0.13.jar"
          jdbc_driver_class => "com.mysql.jdbc.Driver"
          jdbc_connection_string => "jdbc:mysql://remoteIP:3306/kim?serverTimezone=Asia/Seoul"
          jdbc_user => "user"
          jdbc_password => "1234"
          schedule => "*/1 * * * *" #실행빈도 (1분마다)
          #쿼리 매개변수 
          parameters => {"age" => "15"}
          statement => "SELECT * FROM users WHERE age >= :age"
      }
  }
  ```
  
### stdin
standard input로부터 events를 읽음

-----------------------------------
### 테스트 해볼 plugin
* redis
* http
* github
* syslog
* heartbeat
