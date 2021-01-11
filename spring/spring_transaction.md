# Spring Transaction
## Transaction

### Transaction 설정
Spring에서 트랜잭션을 설정하는 방법은 두가지가 있다.
 * 선언적 트랜잭션
 * Javaconfig(개인적으로 주로 이거씀)

#### 선언적 트랜잭션
선언적 트랜잭션은 XML 설정파일에 트랜잭션 설정을 하는 방법이다. 아래와 같은 방식으로 선언하여 사용함.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans 
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop 
		http://www.springframework.org/schema/aop/spring-aop.xsd
		http://www.springframework.org/schema/tx 
		http://www.springframework.org/schema/tx/spring-tx.xsd">

	<!-- Transaction 설정 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
 		<property name="dataSource" ref="dataSource"/>
 	</bean>

 	 	 
 	<!-- Transaction을 위한 AOP 설정 -->
 	<aop:config proxy-target-class="true">
		<aop:pointcut id="servicePublicMethod" expression="execution(public * com.freehoon.web.board..*(int))" />
		<aop:advisor advice-ref="txAdvice" pointcut-ref="servicePublicMethod" />
	</aop:config>


	<!-- 선언적 Transaction 설정  -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="getBoardContent" rollback-for="Exception" />
		</tx:attributes>    
 	</tx:advice> 	
</beans>
```

#### JavaConfig
아래와 같이 설정한다.
```java
@Configuration
@EnableTransactionalManagement
public class DataSourceConfig {
  @Bean
  public DataSourceTransactionManager txManager() {
    return new DataSourceTransactionManager(getDataSource());
  }
}
```
#### Transaction 적용
Transaction을 적용할 메소드에 `@Transactional(rollbackFor = Exception.class)`와 같이 어노테이션 명시해주면 된다.

### multi db transaction
- 요구사항: DB connection이 2개. db1에서 insert를 완료하고 db2에서 insert를 하는 도중 exception이 발생하면 db1, db2 로직 모두 rollback되어야함.
- 해결 방법: `spring-data-commons`에서 제공해주는 `ChainedTransactionManager`를 사용

#### 의존성 추가
```
dependencies {
    compile('org.springframework.data:spring-data-commons')
}
```
