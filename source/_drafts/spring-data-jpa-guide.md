---
title: Spring Data JPA 初学习
date: 2019-07-19 20:47:34
tags:
    - Spring Data JPA
    - Java
categories:
    - Programming
---

JPA，全称 Java Persisting Data，是 Java 持久化处理数据的一种方式。

今天初次接触了 Spring Data JPA 的使用，主要通过 Spring in Action 的 3.2 Persisting data with Spring Data JPA 和 Spring 官网的 [Accessing Data With JPA](https://spring.io/guides/gs/accessing-data-jpa/) Guide，了解了 JPA 的基本使用方式和原理。

下文主要以 Spring 官网的 Guide 为例进行解释，内容基本与该 Guide 相同。

<!-- more -->

## 初始化开发环境

通过 Spring 官方提供的 [Spring Initializr](https://start.spring.io/)，进行初始化项目并选择所需组件。

选项如下：

- Project: Maven Project
- Language: Java
- Dependencies:
    - 必选
        - JPA
        - H2 (Embedded Database Supported)
    - 可选
        - DevTools (HotReload etc.)

## 定义一个简单的实体

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    private String firstName;
    private String lastName;

    protected Customer() {}

    public Customer(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    @Override
    public String toString() {
        return String.format(
                "Customer[id=%d, firstName='%s', lastName='%s']",
                id, firstName, lastName);
    }
}
```

这样就有了一个　`Customer` 类并对应了一个数据库实体（表），通过　`@Entity` 注解表明　`Customer` 类是一个 `JPA` 实体。缺省 `@Table` 自定义表名的注解，将映射到一个名称为与该类同名，即名为 `Customer` 的表。

该表有以下属性，即 `Customer` 的属性域。

- id
- firstName
- lastName

其中　`id` 被 `@Id` 注解标注，表示该属性域为这个对象的 `ID`，`@GeneratedValue(strategy=GenerationType.AUTO)` 注解表明该域的值将自动生成。

`firstName` 和 `lastName` 没有注解标记，被假设映射到表中的栏目(Column)。

## 创建简单的查询语句

接下来创建一条简单的查询语句来体验 `JPA` 提供的查询功能。

我们并不需要编写实际的代码，只要声明一个接口继承 `CrudRepository` 接口，并声明需要操作的类以及 `ID` 的类型，之后在声明的接口中根据 `JPA` 提供的方式创建的名称来声明方法，即可完成查询方法的编写。

示例代码如下：

```java
import java.util.List;

import org.springframework.data.repository.CrudRepository;

public interface CustomerRepository extends CrudRepository<Customer, Long> {
    List<Customer> findByLastName(String lastName);
}
```

该示例的名称为 `findByLastName` 表明查询条件(`By`)为 `Customer` 的 `LastName` 与调用是提供的 `lastName` 相等，并返回所有符合条件的 `Customer`。

Spring in　Action 中提到，与 `find` 表示的操作相同，`get` 和 `read` 都表示查询操作。即 `findByLastName`、`readByLastName` 和 `getByLastName` 等价。

`By` 隐含表示 `Equals` 条件，其他类似的操作还有：

- IsAfter, After, IsGreaterThan, GreaterThan
- IsGreaterThanEqual, GreaterThanEqual
- IsBefore, Before, IsLessThan, LessThan
- IsLessThanEqual, LessThanEqual
- IsBetween, Between
- IsNull, Null
- IsNotNull, NotNull
- IsIn, In
- IsNotIn, NotIn
- IsStartingWith, StartingWith, StartsWith
- IsEndingWith, EndingWith, EndsWith
- IsContaining, Containing, Contains
- IsLike, Like
- IsNotLike, NotLike
- IsTrue, True
- IsFalse, False
- Is, Equals
- IsNot, Not
- IgnoringCase, IgnoresCase

完整内容可参考 spring 官方文档 [Query Creation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)

## 创建 Application 类

现在可以创建 `Application` 完成该示例。

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class Application {
	private static final Logger log = LoggerFactory.getLogger(Application.class);

	public static void main(String[] args) {
		SpringApplication.run(Application.class);
	}

	@Bean
	public CommandLineRunner demo(CustomerRepository repository) {
		return (args) -> {
			// save a couple of customers
			repository.save(new Customer("Jack", "Bauer"));
			repository.save(new Customer("Chloe", "O'Brian"));
			repository.save(new Customer("Kim", "Bauer"));
			repository.save(new Customer("David", "Palmer"));
			repository.save(new Customer("Michelle", "Dessler"));

			// fetch all customers
			log.info("Customers found with findAll():");
			log.info("-------------------------------");
			for (Customer customer : repository.findAll()) {
				log.info(customer.toString());
			}
			log.info("");

			// fetch an individual customer by ID
			repository.findById(1L)
				.ifPresent(customer -> {
					log.info("Customer found with findById(1L):");
					log.info("--------------------------------");
					log.info(customer.toString());
					log.info("");
				});

			// fetch customers by last name
			log.info("Customer found with findByLastName('Bauer'):");
			log.info("--------------------------------------------");
			repository.findByLastName("Bauer").forEach(bauer -> {
				log.info(bauer.toString());
			});
			log.info("");
		};
	}
}
```

`@SpringBootApplication` 注解是 `Spring` 和 `Spring Boot` 的基础知识，暂且不解释。主要是 `demo` 方法，该方法会在初始化 `Spring` 项目的时候运行。

由于 `@Bean` 注解，会自动提供所需的 `repository`，该对象与 `Customer` 表对应。
首先调用 `repository` 的 `save` 为该表提供一些初始化值以供后续查询操作查询。

之后是三次查询

1. 通过 `JPA` 提供的 `findByAll()` 查找所有的 `Customer`
2. 通过 `JPA` 提供的 `findById()` 查找符合提供的 `id` 的 `Customer`
3. 通过先前编写的 `findByLastName`　查找符合提供的 `lastName` 的 `Customer`

以上查询结果皆通过　`log` 输出成为日志。

## 运行

由于是 `Maven` 项目，可以通过 `./mvnw spring-boot:run` 的方式运行。

最后看到类似一下输出

~~~
Customers found with findAll():
-------------------------------
Customer[id=1, firstName='Jack', lastName='Bauer']
Customer[id=2, firstName='Chloe', lastName='O'Brian']
Customer[id=3, firstName='Kim', lastName='Bauer']
Customer[id=4, firstName='David', lastName='Palmer']
Customer[id=5, firstName='Michelle', lastName='Dessler']

Customer found with findById(1L):
---------------------------------
Customer[id=1, firstName='Jack', lastName='Bauer']

Customer found with findByLastName('Bauer'):
--------------------------------------------
Customer[id=1, firstName='Jack', lastName='Bauer']
Customer[id=3, firstName='Kim', lastName='Bauer']
~~~

如果想在运行的过程中实际查看 `H2` 数据库中的内容，只要在浏览器中输入 `server/h2-console`，其中 `server` 一般为 `localhost:8080`，即 `Spring` 项目查看的本地服务地址，所以，`H2` 地址一般是 `localhost:8080/h2-console`。 即可跳出登录界面，然后将 `JDBC URL` 的值改为 `jdbc:h2:mem:testdb`，然后连接即可连接到 `H2` 数据库。

## 总结

通过该示例可以看到 `JPA` 大大简化了项目访问数据库的代码。

## 参考

1. [Accessing Data With JPA](https://spring.io/guides/gs/accessing-data-jpa/)
2. Spring in Action, 5th Edition