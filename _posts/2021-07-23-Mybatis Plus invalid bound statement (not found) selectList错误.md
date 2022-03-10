# Mybatis Plus invalid bound statement (not found) selectList错误

---

因为之前使用了mybatis集成到springBoot项目中，在之后集成mybatis plus就会出现如上错误，该错误发生的第一件事就该确定pom.xml中是否存在mybatis的依赖，还存在将其删除，如果不存在了，则使用MybatisSqlSessionFactoryBean即可解决；自定义mysqlsqlsessionfactoryBean如下所示:

```java
@Configuration
public class MyBastisConfig {
    @Bean("sqlSessionFactory")
    @Primary
    public SqlSessionFactory sqlSessionFactory(@Autowired @Qualifier("dataSource") DataSource dataSource) throws Exception {

        MybatisSqlSessionFactoryBean sqlSessionFactoryBean = new MybatisSqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mappers/*.xml"));
        
        return sqlSessionFactoryBean.getObject();
    }
}
```

