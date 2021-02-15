---

---

# Spring Boot

## catalog



## start

### creat project

Go to [click here](https://start.spring.io/) unzip the file, now this file is the project root, use IntelliJ to open it, then a new file called .gradle will be created

Make sure your project is under java 1.8 by check "File - Project Structure" in IntelliJ.

![project structure](img/project_structure.png)



### add dependencies

Add dependencies in build.gradle from [here](https://mvnrepository.com/artifact/org.springframework.boot), dependencies are here:

- `spring-boot-web-starter`: develop base on RESTful services;
- `spring-boot-starter-security`: access-control features;
- `io.jsonwebtoken`: for token;
- `spring-boot-starter-data-jpa`: for relational database access, if we use mongodb, don't include this one;
- `spring-boot-starter-data-mongodb`: for mongo database access;
- `aws-java-sdk`: aws related development.

Every time when you add new dependency, you will have a gradle icon:

<img src="img/load_dependencies.png" alt="load icon" style="zoom:50%;" />



click on the icon to load the new dependency.

