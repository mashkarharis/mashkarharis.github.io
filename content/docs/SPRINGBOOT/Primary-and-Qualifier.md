[
![](/images/Screenshot%202025-03-10%20123810.png)]
Let's say we have a *PaymentProcessorService *in our application. This service needs to send notifications to users and for this, the class requires the *NotificationManagementService* type instance to be injected.
The *NotificationManagementService *has three implementations: *SMSNotificationManagementService*, *PushNotificationManagementService*, and *EmailNotificationManagementService*. Based on the use case, when we use *PaymentProcessorService*, we need to inject SMS, Push, and/or Email notification management services accordingly. 
But will the following code work?
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25 |@ServicepublicclassSMSNotificationManagementServiceimplements NotificationManagementService \{\}@ServicepublicclassPushNotificationManagementServiceimplements NotificationManagementService \{\}@ServicepublicclassEmailNotificationManagementServiceimplements NotificationManagementService \{\}// INJECTING@ServicepublicclassPaymentProcessorService\{privatefinal NotificationManagementService notificationManagementService;@AutowiredpublicPaymentProcessorService\(NotificationManagementService notificationManagementService\)\{this.notificationManagementService= notificationManagementService;\}\} |
|---|---|
Answer is ***No***
This will throw a *NoUniqueBeanDefinitionException *because Spring Boot finds multiple beans registered for *NotificationManagementService *and cannot decide which one to use.
So, it's obvious that we need a way to tell the application which bean to use and when. That's where the annotations *@Primary* and *@Qualifier* come in.
* @Primary: Use this on the bean that should be injected by default.
* @Qualifier\("<bean\_name>"\): Use this to instruct the application to inject a specific bean other than the default. We need to provide the registered bean name as the @Qualifier parameter. \(By default, Spring takes the class name, converts the first letter to lowercase and use it as bean name\)

Now, let's look at the above code again, but this time using these annotations:
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12 |@ServicepublicclassSMSNotificationManagementServiceimplements NotificationManagementService \{\}@Service@PrimarypublicclassPushNotificationManagementServiceimplements NotificationManagementService \{\}@ServicepublicclassEmailNotificationManagementServiceimplements NotificationManagementService \{\} |
|---|---|
#### Injecting Beans with @Primary and @Qualifier
By default, PushNotificationManagementService will be injected because of @Primary
| 1
2
3
4 |@AutowiredpublicPaymentProcessorService\(NotificationManagementService notificationManagementService\)\{this.notificationManagementService= notificationManagementService;\} |
|---|---|
To inject SMSNotificationManagementService, we use @Qualifier
| 1
2
3
4
5
6 |@AutowiredpublicPaymentProcessorService\(@Qualifier\("sMSNotificationManagementService"\) NotificationManagementService notificationManagementService
\)\{this.notificationManagementService= notificationManagementService;\} |
|---|---|
To inject EmailNotificationManagementService, we specify it with @Qualifier
| 1
2
3
4
5
6 |@AutowiredpublicPaymentProcessorService\(@Qualifier\("emailNotificationManagementService"\) NotificationManagementService notificationManagementService
\)\{this.notificationManagementService= notificationManagementService;\} |
|---|---|
By this way we reduce ambiguidy and define which bean to be injected explicitly. Also if we used custom bean names, we have to use those names within Qualifier annotation to define the bean to be injected. 
Happy Coding 🙌
