# 多数据源

本项目中多数据源的支持由[MybatisPlus Dynamic Datasource](https://mybatis.plus/guide/dynamic-datasource.html)提供。

## 使用方法：

以dynamic-datasource与druid结合为例:

1. pom中加入dynamic-datasource的依赖

   ```markup
   <!-- dynamic-datasource -->
   <dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
    <version>${dynamic.datasource.version}</version>
   </dependency>
   ```

2. 启动Application中将DruidDataSourceAutoConfigure加入到exclude

   ```java
   @SpringBootApplication(exclude = {SecurityAutoConfiguration.class, DruidDataSourceAutoConfigure.class})
   ```

3. 配置文件application-profile.yml中加入多个数据源配置  
   主库为master,其它库名字自定义如salve\_name

   ```yaml
   # Spring
   spring:
   datasource:
    druid:
      stat-view-servlet:
        enabled: true
        url-pattern: /druid/*
        #login-username: admin
        #login-password: admin
      filter:
        stat:
          log-slow-sql: true
          slow-sql-millis: 1000
          merge-sql: false
        wall:
          config:
            multi-statement-allow: true
    dynamic:
      druid:
        filters: stat
      datasource:
        master:
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://127.0.0.1:3306/onex?allowMultiQueries=true&useUnicode=true&characterEncoding=UTF-8&useSSL=false
          username: onex
          password: onex
          druid:
            initial-size: 5
        slave_name:
          driver-class-name: com.mysql.jdbc.Driver
          url: jdbc:mysql://127.0.0.2:3406/slave?allowMultiQueries=true&useUnicode=true&characterEncoding=UTF-8&useSSL=false
          username: slave
          password: slave
          druid:
            initial-size: 6
   ```

4. 使用 @DS 切换数据源  
   master库作为默认库读取方式不变,其它库只要在实体对应ServiceImpl类定义或者方法定义加入@Ds注解即可。同时存在方法注解优先于类上注解。

   ```java
   import com.baomidou.dynamic.datasource.annotation.DS;
   @Service
   @DS("slave_name")
   public class AreaServiceImpl implements AreaService {}
   ```

## Ref.

1. [dynamic-datasource](https://mybatis.plus/guide/dynamic-datasource.html)

