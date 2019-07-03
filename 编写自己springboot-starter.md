# 编写自己的Spring Boot自动装配
------------------------------------------------
## 以kaptcha为例
* kaptcha-springboot-starter
## 1.通过IDEA创建springboot项目
 * 删除resources下面的application配置文件
 * 在resources新建META-INF文件夹
 * 在此文件夹下创建spring.factories文件
 * 删除src/main/java文件夹下的XXXapplication文件
 * 删除pom.xml 的bulid标签
### 最后的文件目录如下

* kaptcha
   * src
     * main
       * java
          * com.kaptcha.start
       * resources
          * META-INF  
             * spring.factories
  
--------------------------------------------------------------
## pom.xml配置
* 添加依赖项
```
<!--生成metadata.json 为IDE配置application.yml提供支持-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
<!--自动装配类-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
            <optional>true</optional>
        </dependency>
<!--验证码jar-->
        <dependency>
            <groupId>com.github.penggle</groupId>
            <artifactId>kaptcha</artifactId>
            <version>2.3.2</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
```          
* 修改项目的groupid等
```
    <groupId>com.kaptcha</groupId>
    <artifactId>kaptcha-spring-boot-starter</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    <description>Kaptcha Springboot Starter</description>
```
-----------------------------------------------------------------------

## src/main/java 下目录结构

  * configuration
     * KaptchaConfiguration
  * properties
     * KaptchaProperties
---------------------------------------------------------------------
## 创建属性配置类KaptchaProperties

```
@Component
@Setter
@Getter
@ToString
@ConfigurationProperties(prefix ="kaptcha")
public class KaptchaProperties {

    private String border = "no";
    private String borderColor = "black";
    private Integer borderThickness = 1;
    private Integer width = 140;
    private Integer height = 40;
}
```
--------------------------------------------------
##创建自动装配类
```
@Configuration
@ConditionalOnClass(DefaultKaptcha.class)
@EnableConfigurationProperties(KaptchaProperties.class)
public class KaptchaConfig implements DisposableBean {

    private final KaptchaProperties kaptchaProperties;

    public KaptchaConfig(KaptchaProperties properties) {
        this.kaptchaProperties = properties;
    }

    @Bean
    @ConditionalOnMissingBean
    public DefaultKaptcha defaultKaptcha(){
        DefaultKaptcha defaultKaptcha = new DefaultKaptcha();
        Config config = new Config(setProperties());
        defaultKaptcha.setConfig(config);
        return defaultKaptcha;
    }

    @Override
    public void destroy() throws Exception {

    }

    public Properties setProperties(){

        Properties properties = new Properties();
        properties.setProperty("kaptcha.border",
        this.kaptchaProperties.getBorder());

        properties.setProperty("kaptcha.border.color",
        this.kaptchaProperties.getBorderColor());

        properties.setProperty("kaptcha.image.width",
        this.kaptchaProperties.getWidth().toString());

        properties.setProperty("kaptcha.image.height",
        this.kaptchaProperties.getHeight().toString());

        properties.setProperty("kaptcha.border.thickness",
        this.kaptchaProperties.getBorderThickness().toString());

    }
}
```
--------------------------------------
### 在自动装配中常用的注解
* @ConditionalOnBean:当容器中有指定的Bean的条件下  
* @ConditionalOnClass：当类路径下有指定的类的条件下  
* @ConditionalOnExpression:基于SpEL表达式作为判断条件  
* @ConditionalOnJava:基于JVM版本作为判断条件  
* @ConditionalOnJndi:在JNDI存在的条件下查找指定的位置  
* @ConditionalOnMissingBean:当容器中没有指定Bean的情况下  
* @ConditionalOnMissingClass:当类路径下没有指定的类的条件下  
* @ConditionalOnNotWebApplication:当前项目不是Web项目的条件下  
* @ConditionalOnProperty:指定的属性是否有指定的值  
* @ConditionalOnResource:类路径下是否有指定的资源  
* @ConditionalOnSingleCandidate:当指定的Bean在容器中只有一个，或者在有多个Bean的情况下，用来指定首选的Bean @ConditionalOnWebApplication:当前项目是Web项目的条件下  
## 通过maven install 打包
即可通过配置application.yml的方式实现kapacha的自动装配

## [Use](https://github.com/netdied/kaptcha-springboot-starter/blob/master/README.md) 