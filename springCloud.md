# xspring cloud相关笔记

## 1.1 spring cloud主要用到的框架

- 服务发现--Eureka
- 服务调用--Feign
- 熔断器--Hystrix
- 服务网关--Zuul
- 分布式配置--Spring Cloud Config
- 消息总线--Spring Cloud Bus



## 1.2 部署中遇到的小问题

### 1.maven

- 在引用SpringCloud模块，例子中使用到了<dependencyManagement>标签，用于控制子模块引用的时候版本的问题，不再使用<version>标签，导致版本引用的混乱

- <dependencyManagement>中的<type>标签表示的是通过什么方式到子模块中，默认情况下是jar，这里修改成了pom形式，<scope>标签表示的是作用域范围，一般填写的是import，例子：

  ```xml
  父项目创建的标签
  <dependencyManagement>
  		<dependencies>
  			<dependency>
  				<groupId>junit</groupId>
  				<artifactid>junit</artifactId>
  				<version>4.8.2</version>
  			</dependency>
  			<dependency>
  				<groupId>log4j</groupId>
  				<artifactid>log4j</artifactId>
  				<version>1.2.16</version>
  			</dependency>
  		</dependencies>
  	</dependencyManagement>
  ```

  ```xml
  子模块引用的示例：
  <dependencyManagement>
  	<dependencies>
  		<dependency>
  			<groupId>com.test.sample</groupId>
  			<artifactid>base-parent1</artifactId>
  			<version>1.0.0-SNAPSHOT</version>
  			<type>pom</type>
  			<scope>import</scope>
  		</dependency>
  	</dependencies>
  </dependencyManagement>
  <dependencies>
  <dependency>
  	<groupId>junit</groupId>
  	<artifactid>junit</artifactId>
  </dependency>
  <dependency>
  	<groupId>log4j</groupId>
  	<artifactid>log4j</artifactId>
  </dependency>
  </dependencies>
  ```

  

## 1.3 创建Eureka注册组件

### 1.3.1 添加application.yml

```yaml
server:
port: 6868 #服务端口
eureka:
client:
registerWithEureka: false #是否将自己注册到Eureka服务中，本身就是所有无需注册
fetchRegistry: false #是否从Eureka中获取注册信息
serviceUrl: #Eureka客户端与Eureka服务端进行交互的地址
defaultZone: http://127.0.0.1:${server.port}/eureka/
```

- 配置完成后，在启动类上添加@EnableEnkuraServer注解，运行启动类，服务器就会运行



## 1.4 注册Eureka的Client

- 给需要注册到Eureka的模块，在application.yml上添加：

  ```yaml
  eureka:
  client:
  service‐url:
  defaultZone: http://localhost:6868/eureka
  instance:
  prefer‐ip‐address: true
  ```

  

- 然后给每个服务的启动类添加@EnableEurekaClient，模块就会注册到Eureka的服务器



## 1.5 使用Feign组件实现各个服务间的调用

- 添加Feign的依赖：

  ```xml
  <dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring‐cloud‐starter‐openfeign</artifactId>
  </dependency>
  ```

  

- 在启动类上添加@EnableDiscoveryClient和@EnableFeignClients
  - @EnableDiscoveryClient在Dalson以及更为早期的版本中，是用于注册Eureka组件的注册的，当时的@EnableEurekaClient也是同样的效果，但是现在版本在Dalson之后，@EnableDiscoveryClient被放入到了springboot的自动配置当中，只要开启了自动配置的话，springboot就会自动启动服务注册发现的功能
  - @EnableFeignClients
    - 早期适合上面的注解作用是相似的，而且只为Feign的组件专门使用，后面更新之后，spring整合到了common上面，使得这个东西已经若有若无，加不加都没有什么区别

- 再在启动类上添加@EnableFeignClients,使得其成为Feign的客户端
- 在某个地方创建一个interface ，并且将添加@FeignClient的注解，例子：
  - @FeignClient注解用于指定从哪个服务中调用功能 ，注意 里面的名称与被调用的服务
    名保持一致，并且不能包含下划线
  - @RequestMapping注解用于对被调用的微服务进行地址映射。注意 @PathVariable注
    解一定要指定参数名称，否则出错

- 实际上这里创建的LabelClient并不需要@Component来注册，同样可以使用@AutoWired进行调用，因为本质上这时网络间的调用，即使idea提示报错也可以不用理会

```java
@FeignClient(value= "tensquare‐base"，path="/label")//引用某个模块在application.yml中spring.application.name的属性值，该值就是微服务的名字，这里的path相当于是控制器类上的父级映射路径
public interface LabelClient {
    //这里的方法映射了tensquare‐base模块的findById的方法，路经一定要写完整
@RequestMapping(value="/{id}", method = RequestMethod.GET)
public Result findById(@PathVariable("id") String id);//这里的注解默认为变量名不再生效，需要添加值来映射路径的变量值
}
```

