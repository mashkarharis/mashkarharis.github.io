[
![](/images/java%20flow.drawio.png)]
### RetentionPolicy.SOURCE
Annotations are removed after compilation.
✅ Available only during compilation.
❌ Not present in the .class file.
❌ Not available at runtime.
Use Case: Lombok \(@Getter, @Setter\)
### RetentionPolicy.CLASS
Annotations are kept in the .class file but removed at runtime.
✅ Available during compilation.
✅ Exists in the compiled .class file.
❌ Not available at runtime.
Use Case: Useful when doing byte code\-level post\-processing
### RetentionPolicy.RUNTIME
Annotations are available even after compilation and class loading.
✅ Exists in the .class file.
✅ Available at runtime via reflection.
Use Case: Spring \(@Service, @Autowired\), Hibernate \(@Entity, @Table\).
Happy Coding 🙌
