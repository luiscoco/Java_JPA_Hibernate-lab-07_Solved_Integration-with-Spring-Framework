# Java_JPA_Hibernate-lab-07_Solved

## 1. Exercise

Integration with Spring Framework

**Goal**

Learn facilities the Spring Framework provides for supporting JPA infrastructure.

**Subject**

You have an entity Company and service class CompanyService for managing the Company entity. 

You need to do the following:

o	Implement logic of CompanyService class using Spring Framework facilities to inject reference to EntityManager

o	Provide transaction configuration for methods of CompanyService (declarative transaction management)

o	Configure Spring Framework context with all needed objects (JDBC data source, JPA EntityManagerFactory, Transaction manager, CompanyService, etc.)

**Description**

1.	Open module jpa-lab-07

**Implementing CompanyService**

2.	Open class edu.jpa.service.CompanyService

3.	Specify object stereotype @Repository for CompanyService class (optional, needed if component-scan is used)

```
@Repository
public class CompanyService {
. . .
}
```

4.	Define the class fields of type EntityManager to inject the reference to entoty manager:

```
@PersistenceContext
private EntityManager em;
```

5.	Implement init() method. See the code sample below:

```
public void init() {
    String[] companies = {"Microsoft", "IBM"};
    for (String name : companies) {
        Company company = new Company();
        company.setName(name);
        em.persist(company);
    }
}
```

6.	Specify transaction attributes via using @Transactional annotation:

```
@Transactional(propagation = Propagation.REQUIRES_NEW)
```

7.	Implement getCompany(int)::Company method. See the code sample below:

``` 
public Company getCompany(int id) {
    return em.find(Company.class, id);
}
```

8.	Specify transaction attributes via using @Transactional annotation:

```
@Transactional(propagation = Propagation.REQUIRES_NEW, readOnly = true)
```

**Spring Framework configuration**

9.	Open application-context.xml file

10.	Enable loading configuration parameters from application-context.properties file:

```
<context:property-placeholder location="application-context.properties"/>
```

11.	Enable annotations support for dependencies injection and configuration:

```
<context:annotation-config/>
```

12.	Define JDBC DataSource that will be used to get JDBC connection to database:

```
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
```

13.	Define EntityManagerFactory factory bean with all JPA configuration included (persistence unit name, dialect, reference to data source, properties, etc.):

```
<bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
    <property name="persistenceUnitName" value="persistenceUnits.lab07" />
    <property name="dataSource" ref="dataSource"/>
    <property name="jpaProperties">
        <props>
            <prop key="hibernate.dialect">org.hibernate.dialect.MySQL5Dialect</prop>
            <prop key="hibernate.show_sql">true</prop>
            <prop key="hibernate.format_sql">true</prop>
            <prop key="hibernate.hbm2ddl.auto">create</prop>
        </props>
    </property>
    <property name="jpaDialect">
        <bean class="org.springframework.orm.jpa.vendor.HibernateJpaDialect"/>
    </property>
</bean>
```

14.	Define transaction management strategy bean:

```
<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
    <property name="entityManagerFactory" ref="entityManagerFactory" />
    <property name="dataSource" ref="dataSource" />
</bean>
```

15.	Define CompanyService as sprint bean to let Spring Framework inject the needed dependencies: 

```
<bean class="edu.jpa.service.CompanyService"/>
```

**Running**

16.	Open file META-INF/persistence.xml and look how simple it becomes (no properties, no connection info).

17.	Open class edu.jpa.Launcher and analyze it.

18.	Run class edu.jpa.Launcher and analyze the output.

## 2. Solution



## 3. How to Run the Application in VSCode



## 4. Application Output (MySQL database)
