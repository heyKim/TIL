# Spring Batch 
* Spring Batch는 스케줄러가 아니고 배치는 '일괄처리'를 의미함.
* Enterprise Scheduler: Quartz, Tivoli, Control-M, etc 
* (특정한 시간에 내가 원하는 일을 자동으로 시키는게 스케줄러의 역할)
 
## Spring Batch 컴포넌트 
* Job: Batch에서 실행 가능한 실행 단위, 배치 작업 단위 
* JobLauncher : Job을 실행하는 책임이 있다. JobRepository를 통해 Job, Step, Item* 등을 생성하고 조립. 
* JobRepository: 배치 처리 정보를 담고 있는 매커니즘 
    * 어떤 Job이 실행되었고 몇번 실행되고 언제 끝났는지 등 배치 처리에 대한 메타데이터를 저장함 
    * DB 또는 어딘가에 저장된 Job, Step 등을 조회(select) 또는 생성(insert), 수정(update). 
* Step : Job은 1개 이상의 Step을 포함(1:N)할 수 있고, 하나의 큰 실행 단위가 Job이라면 Step은 Job의 통제를 받으며 Job 안에서 실행 가능한 Job보다 작은 실행 단위. 실질적인 배치 처리를 정의한다고 보면 됨 
* ItemReader : 하나의 Step은 0~1개의 ItemReader를 포함하며, 데이터를 어디선가 (DB, file, memory 등) 조회하는 역할. 
* ItemProcessor : 하나의 Step은 0~1개의 ItemProcessor를 포함하며, ItemReader를 통해 조회한 데이터를 중간에서 가공하는 역할. 
* ItemWriter : 하나의 Step은 0~1개의 ItemWriter를 포함하며, ItemProcessor를 통해 가공된 데이터를 Write하는 역할. 이름이 Writer 지만, 쓰기만 가능한 건 아니다. 
 
### ItemReader 
Spring Batch 에서 제공하는 ItemReader 유형은 2가지 
1. File 형식의 데이터 Reader (txt, csv, xml, json) 
    * txt, csv => FlatFileItemReader 
    * xml => Spring OXM을 통해 XML 읽음 
    * json => JsonItemReaderBuilder 
    * multi 파일 => MultiResourceItemReader 
 
2. DB접근을 통한 데이터 Reader 
* Spring의 JdbcTemplate은 분할 처리를 지원하지 않기 때문에(쿼리 결과를 그대로 반환해서) 개발자가 직접 limit, offset을 사용하는 작업이 필요하다. 
* Spring Batch는 쿼리 결과를 직접 반환하는 Spring의 JdbcTemplate 문제를 해결하기 위해 2개의 타입을 지원한다. 
    1. Cursor - 실제로 JDBC ResultSet의 기본 기능이다. 
        * ResultSet이 open 될 때마다 next() 메소드가 호출 되어 Database의 데이터가 반환된다. 
        * 이를 통해 필요에 따라 Database에서 데이터를 Streaming할 수 있다. 
 
    2. Paging - 페이지라는 Chunk로 Database에서 데이터를 검색한다. 
        * 페이지 단위로 한번에 데이터를 조회해오는 방식이다.  
 
## 기타 용어 정리 
* JobBuilderFactory: 여러 빌더를 통합하여 처리하는 공장 개념. JobBuilderFactory의 get() 메서드를 호출하여 원하는 Job을 쉽게 만들 수 있다. 
* JobInstance: 배치에서 Job이 실행될 때 하나의 Job의 논리적인 실행 단위 
    * JobRepository를 통해 조회/생성되며, 단순히 Job의 name과 id 필드로 선언되어 있을 뿐 
    * 여러 개의 JobExecution(JobInstance에 대한 한번의 실행을 나타내는 객체)을 가지고 있을 수 있다. 
    * 오늘 Job을 실행했는데 실패했다면 다음날 동일한 JobInstance를 가지고 또 실행합니다. Job 실행이 실패하면 JobInstance가 끝난 것으로 간주하지 않기 때문입니다.**머선 말?** 
    * JobInstance는 어제의 실패한 JobExecution과 오늘의 성공한 JobExcution 두 개를 가지게 됩니다. 
* JobExecution: JobInstance에 대한 한 번의 실행을 나타내는 객체 
* JobParameters: Job이 실행될 때 필요한 파라미터을 Map 타입으로 저장하는 객체 
    * 하나의 Job이 생성될 때, 시작 시간등의 정보를 파라미터로 해서 하나의 JobInstance를 생성해서 JobInstance와 JobParameters는 1:1 관계를 유지한다. 
    * 가능한 파라미터 타입: String, Long, Date, Double 
* JobLauncher: Job. JobParamerters와 함께 배치를 실행하는 인터페이스입니다. 
* JobFlowBuilder:  
 
## Spring Batch 관련 Annotation 
* `@StepScope` 
    * Spring Batch 컴포넌트 (Tasklet, ItemReader, ItemWriter, ItemProcessor 등)에 `@StepScope`를 쓰면 Step의 실행시점에 해당 컴포넌트를 Spring Bean으로 생성한다. 
* `@JobScope` 
    * Job 실행 시점에 Spring Bean으로 생성한다. 즉, Bean의 생성 시점을 지정된 Scope가 실행되는 시점으로 지연시킨다. 
 
 
## Batch Use case 
* 일매출 집계: 전날 매출 데이터로 집계데이터 생성 
* DB, file, queue 등에서 대량 record를 읽어오는 작업 
* 가입한 회원 중 1년이 지나도록 로그인한 내역이 없는 경우 휴면회원으로 전환하는 배치 처리 
 
## Spring Batch 관리 도구 
* Cron 
* 클라우드 서버리스 
    * 이를테면 AWS의 Lambda 에 배치 코드를 등록하고, AWS CloudWatch에서 스케줄링 실행을 하도록 구성하는 방식입니다. 
* API 기반의 직접 만든 관리자 페이지 
* Spring Batch Admin*Deprecated) 
    * Spring Cloud Data Flow 로 전환 
* Quartz를 이용한 관리자 페이지 
    * Scheduler 역할로 Quartz를 사용하고 그에 대한 UI 대시보드를 직접 만드는 경우입니다. 
* CI 서비스 (Jenkins / Teamcity 등등) 


## Reference 
* [Spring Batch Tutorial](https://www.baeldung.com/introduction-to-spring-batch) 
* [2. Spring Batch의 ItemReader - File Item Reader](https://renuevo.github.io/spring/batch/spring-batch-chapter-2/) 
* [엄청 deep한 내용도 있음](https://n1tjrgns.tistory.com/159)