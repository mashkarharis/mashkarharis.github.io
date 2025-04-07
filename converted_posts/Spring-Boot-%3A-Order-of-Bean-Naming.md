[
![](../fetched_images/Screenshot 2025-03-11 113739.png)]
 
In this article, we will explore how Spring Boot resolves ambiguity and determines bean names in the application context. Spring Boot follows these rules in order to determine the bean names.
#### 1. Explicit Name in @Bean Annotation
First Spring Boot checks whether bean name is defined in @Bean annotation as follows. If so it will be used as Bean name to register in the context.
| 1
2
3
4 |@Bean\(name ="myDemoService"\)public MyService initMyService\(\)\{returnnewMyService\(\);\} |
|---|---|
Bean Name : <u>myDemoService</u>
<u>
</u>
#### 2. Explicit Name in @Component @Controller etc. Annotation
If not, Spring Boot checks whether bean name is defined in annotation like @Component, @Service, etc. as follows. If so it will be used as Bean name to register in the context.
| 1
2
3
4 |@Service\("sampleService"\)classMyService\{\} |
|---|---|
Bean Name : <u>sampleService</u>
#### 3. Method Name Where @Bean Used
If not, Spring Boot checks whether object is initialized through @Bean \(this time no explicit name within bean annotation given\) method, and use that method name as Bean name to register in the context.
| 1
2
3
4 |@Beanpublic MyService mySimpleService\(\)\{returnnewMyService\(\);\} |
|---|---|
Bean Name : <u>mySimpleService</u>
#### 4. Class Name with First Letter Lowercased
If not, Spring Boot uses class name with first letter lowercased as Bean name in the context.
| 1
2
3
4 |@ServiceclassEmailService\{\} |
|---|---|
Bean Name : <u>emailService </u>
#### 5. Fully Qualified Name in Case of Conflicts
When using class name there can be some classes with same name but in different packages. For example.
| 1
2 |com.example.packageone.EmailService
com.example.packagetwo.EmailService |
|---|---|
In that case it uses Fully Qualified Names as bean name to resolve conflicts as follows,
Bean Names : <u>com.example.packageone.EmailService</u>, <u>com.example.packagetwo.EmailService</u>
Note : This is just an general order of Bean naming. There are some special cases where naming and its order can be different than above.
Happy Coding 🙌
