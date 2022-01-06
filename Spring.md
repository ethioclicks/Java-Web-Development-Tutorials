# Spring tutorial
## Basic Spring Application with Maven

### How to create new maven project for spring tutorial
Open your IntelliJ IDE and create new project as follows
![image](screenshots/spring-screenshots/1-newmavenproject.PNG)
Select your workspace and create
![image](screenshots/spring-screenshots/2-projectname.PNG)

Since maven is using older version of java by default, we have to add java 8 plugin to our maven (if we use latest Jave we have to add that version plugin) as follows.

```
<build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
```
![image](screenshots/spring-screenshots/3-addjava8plugin.PNG)


### Add entity class
public class Speaker {

    String firstName;
    String lastName;

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
}

### add an interface 
import java.util.List;

public interface MockRepository {
    List<Speaker> getSpeakers();
}

### implement an interface
import java.util.ArrayList;
import java.util.List;

public class HibernateRepository implements MockRepository {
    private ArrayList<Speaker> speakers;

    HibernateRepository() {
        speakers = new ArrayList<Speaker>();
        Speaker speaker = new Speaker();
        speaker.setFirstName("Alemu");
        speaker.setLastName("Kebede");
        speakers.add(speaker);
    }
    
    public List<Speaker> getSpeakers() {
        return speakers;
    }
}


### Create Application class and run the application
#### Implementing the repository
public class Application {
    public static void main(String[] args) {
        MockRepository repository = new HibernateRepository();
        System.out.println("Size: " + repository.getSpeakers().size());
        System.out.println("First name: " + repository.getSpeakers().get(0).getFirstName());
    }
}

#### Out put
![image](screenshots/spring-screenshots/4-output.PNG)

### adding another implementation for mock H2 DB
import java.util.ArrayList;
import java.util.List;

public class H2Repository implements MockRepository {
    private ArrayList<Speaker> speakers;

    H2Repository() {
        speakers = new ArrayList<Speaker>();
        Speaker speaker = new Speaker();
        speaker.setFirstName("Alemu from h2");
        speaker.setLastName("Kebede from h2");
        speakers.add(speaker);
    }

    public List<Speaker> getSpeakers() {
        return speakers;
    }
}

### run the app again and will get another result

public class Application {
    public static void main(String[] args) {
        MockRepository repository = new H2Repository();
        System.out.println("Size: " + repository.getSpeakers().size());
        System.out.println("First name: " + repository.getSpeakers().get(0).getFirstName());
    }
}



#### Out put
This time it is using H2 Repository.
![image](screenshots/spring-screenshots/5-output.PNG)

So far when we want to add new implementation, we should modify the code so that it will use the new implementation. One of the advantages of spring is it allow us to configure the application and based on our configuration it will use the dependency needed. 

Let’s do that on the next section

Next>>>>>>>>>>>>>>>>> using spring for dependency injection

##Spring configuration

###adding spring dependency
Note: you can use existing project for this tutorial or you can copy and rename it to new project name to use for this section demo. We are using new project name called “confrenceapp-java”

In order to use spring in our project, we need to add spring library to our project. One way of adding spring in our project is we can manually download the jar and add it to our class path, or we can us maven for downloading and adding it to our class path. 
Add the following dependency in to your pom.xml

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.14</version>
    </dependency>
</dependencies>

### maven install
![image](screenshots/spring-screenshots/6-maveninstall.PNG)



### Java based spring configuration
#### Creating a bean
Lets add a class for spring configuration as follows

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MySpringConfig {

    @Bean
    MockRepository getRepository() {
        return new H2Repository();
    }

}

lets modify our application class to use new configuration that spring will manage and create an object for us instead of we create an object.

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
//         old way of creating an object
//        MockRepository repository = new H2Repository(); 
        
//        spring create an obejct for us
        ApplicationContext context = new AnnotationConfigApplicationContext(MySpringConfig.class);
        MockRepository repository = (MockRepository) context.getBean("getRepository");// we are telling spring which bean to use
        System.out.println("Size: " + repository.getSpeakers().size());
        System.out.println("First name: " + repository.getSpeakers().get(0).getFirstName());
    }
}

we should be able to get the same result as it was before. 
We can use custom name and we can use that name for instantiating an object as follows.

@Bean(name = "myRepository")// we can use custom name
MockRepository getRepository() {


ApplicationContext context = new AnnotationConfigApplicationContext(MySpringConfig.class);
MockRepository repository = (MockRepository) context.getBean("myRepository");


### let recap what we have done on this module
In this section we have used spring to create and manage object for us based on the configuration we provided. This allow us to inject the dependency and specify how the object should be created rather than we created it ourselves. One of the advantages of this is we can use configuration to inject different behaviors. Lets do some modification on configuration and let see the effect.

Change the configuration 

Old configuration:
@Configuration
public class MySpringConfig {

    @Bean(name = "myRepository")// we can use custom name
    MockRepository getRepository() {
        return new H2Repository();
    }

}

New configuration:

@Configuration
public class MySpringConfig {

    @Bean(name = "myRepository")// we can use custom name
    MockRepository getRepository() {
        return new HibernateRepository();
    }

}

Due to our changes the output changed to:
Old output:

Size: 1
First name: Alemu from h2

New output:
Size: 1
First name: Alemu

This way we can change the configuration and alter the behavior of the application by injecting different behaviors.

### There are different way of injecting a dependency in spring such as
- Setter based injection
- Constructor base injection  
As the names indicates setter based injection, inject the dependency using setter method and constructor base injection injects the dependency using constructor. Let see some examples
#### Setter injection
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MySpringConfig {

    MockRepository mockRepository;

    public void setMockRepository(MockRepository mockRepository) {
        this.mockRepository = mockRepository;
    }

    @Bean(name = "myRepository")//We can use custom name
    MockRepository getRepository() {
        this.setMockRepository(new H2Repository());
        return this.mockRepository;
    }

}

#### Constructor injection
 
To demonstrate constructor-based injection let’s create some classes.

public class Engine {
    private int cylinders;
    private String model;

    public Engine(String model, int cylinders) {
        this.model = model;
        this.cylinders = cylinders;
    }

    public int getCylinders() {
        return cylinders;
    }

    public String getModel() {
        return model;
    }
}

public class Transmission {
    private String type;

    public Transmission(String type) {
        this.type = type;
    }

    public String getType() {
        return type;
    }
}

let’s create a car class which requires the above two classes to be constructed.
public class Car {

    private Engine engine;
    private Transmission transmission;

    public Car(Engine engine, Transmission transmission) {
        this.engine = engine;
        this.transmission = transmission;
    }

    public Engine getEngine() {
        return engine;
    }

    public Transmission getTransmission() {
        return transmission;
    }
}

now lets use Engine and Transmission beans in the configuration to construct Car bean.

@Bean
public Engine engine() {
    return new Engine("v8", 5);
}

@Bean
public Transmission transmission() {
    return new Transmission("sliding");
}

@Bean(name = "car")
public Car getCar() {
    return new Car(engine(),transmission());
}

Then lets use this class as follows: 

Car car = context.getBean("car", Car.class);
System.out.println("number of cylinders:"+car.getEngine().getCylinders());
System.out.println("transmission type:"+car.getTransmission().getType());

The out put will be as follows:
number of cylinders:5
transmission type:sliding


### Scops in spring
#### Singleton
The default scop of spring object creation is singleton which means always it give us the same object when we ask spring to create for us. Lets demonstrate by creating two object and changing value on the first object after the creation and lets check if the second object get the modified version or the original version.

ApplicationContext context = new AnnotationConfigApplicationContext(MySpringConfig.class);
MockRepository repository = (MockRepository) context.getBean("myRepository");
repository.getSpeakers().get(0).setFirstName("Alemnesh");

MockRepository repository2 = (MockRepository) context.getBean("myRepository");
System.out.println("Size: " + repository2.getSpeakers().size());
System.out.println("First name: " + repository2.getSpeakers().get(0).getFirstName());

The out put is:
Size: 1
First name: Alemnesh

The output shows that our change after spring created the object for us is reflected when spring created an object of us for the second time. 

#### Prototype
If we want to get new object always when we created an object we need to modify the scop of our bean as follows:

@Bean(name = "myRepository")//We can use custom name
@Scope(value = "prototype")
MockRepository getRepository() {
    return new H2Repository();
}

And lets run the same code and let see the output.
Output:
Size: 1
First name: Alemu from h2

This time we don’t see our changes we made after spring created an object for us. This shows that we are getting a new object.

#### web scops
In addition to Singleton and Prototype we have request session, session and application scop that we can use it on we application

### Autwiring
Auto wiring is another way of creating a bean in spring. In order to use auto wiring we need to annotate the class using stereo types to inject/create a bean. There are different types of stereo types such as 
- @Component
- @Service
- @Repository
- @Component
For example, I can use @Component as follows:

@Component(value = "h2Repository")
public class H2Repository implements MockRepository {

And we can use autwiring
@Configuration
@ComponentScan("com")
public class MySpringConfig {

    @Autowired
    MockRepository mockRepository;

}

It is important to do component scan to find the classes annotated as a component and spring create bean for us. If there are multiple implementations of the interface spring will confuse which implementation of the interface to use for object creation. One way to tell speirng which implementation to use is to spacify the type as followes
@Configuration
@ComponentScan("com")
public class MySpringConfig {

    @Autowired
    MockRepository h2Repository;

}
Or
@Configuration
@ComponentScan("com")
public class MySpringConfig {

    @Autowired
    MockRepository hibernateRepository;

}

Spring will create appropriate object based on type or we can use @Qualifier annotation as follows:

@Configuration
@ComponentScan("com")
public class MySpringConfig {

    @Autowired
    @Qualifier(value = "h2Repository")
    MockRepository mockRepository;

}



### XML Configuration
The other way of configuring spring is to use xml. Xml configuration is the first spring configuration mechanism, it helps to take out the configuration from the code and implement separation of concern. 
In order to demo this concept, we need to create a new project and add the previous classes
 
![image](screenshots/spring-screenshots/7-xml-configuration.PNG)

public class Speaker {

    String firstName;
    String lastName;

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
}



public interface MockRepository {
    List<Speaker> getSpeakers();
}

public class H2Repository implements MockRepository {
    private ArrayList<Speaker> speakers;

    H2Repository() {
        speakers = new ArrayList<Speaker>();
        Speaker speaker = new Speaker();
        speaker.setFirstName("Alemu  h2");
        speaker.setLastName("Kebede  h2");
        speakers.add(speaker);
    }

    public List<Speaker> getSpeakers() {
        return speakers;
    }
}

public class HibernateRepository implements MockRepository {
    private ArrayList<Speaker> speakers;

    HibernateRepository() {
        speakers = new ArrayList<Speaker>();
        Speaker speaker = new Speaker();
        speaker.setFirstName("Alemu hibernate");
        speaker.setLastName("Kebede hibernate");
        speakers.add(speaker);
    }

    public List<Speaker> getSpeakers() {
        return speakers;
    }
}

####adding spring xml configuration
Create xml file in resources folder as followes
Resource->new->file->your file name example(springapp.xml)
Add spring sechema name space as follows(you can find a sample here: 40. XML Schema-based configuration (spring.io) ) 

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- bean definitions here -->
    

</beans>


![image](screenshots/spring-screenshots/8-spring-namespace.PNG)


Now we can create a bean from xml configuration as follows:

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- bean definitions here -->
    <bean name="h2" class="com.repo.H2Repository"></bean>
    <bean name="hibernate" class="com.repo.HibernateRepository"></bean>

</beans>

Then we can test our configuration:


public class Application {

    public static void main(String[] args) {

//        com.repo.MockRepository repository = new com.repo.H2Repository();
        ApplicationContext context = new ClassPathXmlApplicationContext("springapp.xml");

        MockRepository h2repository = (MockRepository) context.getBean("h2");
        MockRepository hibernateRepository2 = (MockRepository) context.getBean("hibernate");
        System.out.println("H2 Repository");
        System.out.println("Size: " + h2repository.getSpeakers().size());
        System.out.println("First name: " + h2repository.getSpeakers().get(0).getFirstName());
        System.out.println("Hibernate Repository");
        System.out.println("Size: " + hibernateRepository2.getSpeakers().size());
        System.out.println("First name: " + hibernateRepository2.getSpeakers().get(0).getFirstName());

    }
}

Output will be as follows:

H2 Repository
Size: 1
First name: Alemu  h2
Hibernate Repository
Size: 1
First name: Alemu hibernate

Generally, we can do all sorts of configurations that we can do using annotation or java-based spring configuration; we can do it using xml configuration. Now a days most of applications uses the first two approaches (java based and annotation-based spring configuration) but xml configuration found on old applications. Further reading on this topic is found here: Spring - Bean Definition (tutorialspoint.com)

Useful annotations 
@Profile :- to decide a code in which environment to run (dev,prod,..)
@PostConstract:- to run a function after spring initialization(construction)


### Spring MVC
