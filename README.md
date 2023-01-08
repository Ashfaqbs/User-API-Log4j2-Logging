_# User_API_Log4j2_Logging
This is a Spring boot project , having user restfull API's , with multiple relationships i.e one to many, and one to one , this project is used to demonstrate the Log4j2 logging of a springboot project

Spring boot by default provides a logging called Logback, disable it.

Step 1:
#Apache Log4J Logging
#Before using it disable default logging provided by sprinboot i.e disable Logback Confid using maven exclude
 i.e go to pom.xml and 
 <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<exclusions>
			<exclusion>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-logging</artifactId>
			</exclusion>
			</exclusions>
</dependency>
    
   and this dependency in pom.xml
 <dependency>
		
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>

Step 2 :
create a log4j2.xml file  in src/main/resource path ans provide and standard pattern for logging
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="DEBUG" monitorInterval="30">
    <Appenders>
        <Console name="LogToConsole" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
		<!-- avoid duplicated logs with additivity=false -->
        <Logger name="com.user" level="debug" additivity="false">
            <AppenderRef ref="LogToConsole"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="LogToConsole"/>
        </Root>
    </Loggers>
</Configuration>


for better and more logging details (optional) we can add this in application.properties file
logging.level.root=INFO
logging.file=app.log
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n



Step 3:
using Loggers in classes


//Apache Log4j imports
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

@RestController
public class MainController {

private static final Logger log = LogManager.getLogger(MainController.class);

@GetMapping("/Users")
	public ResponseEntity<List<User>> getAllUSer()
	{
		List<User> getallUsers=null;
		try {
			getallUsers = this.service.getallUsers();
//			System.out.println(getallUsers);
			log.info(getallUsers.toString());
		} catch (Exception e) {
			log.error( e.getMessage().toLowerCase());
		}
		
		return ResponseEntity.ok(getallUsers);

	}


}


and we can use the loggers in other @Component classes
	
	
	
	
////////logging in diff configurations in springboot
	
	1  Log4j using an XML file with Spring Boot, you can create a log4j2.xml file in the src/main/resources directory. Here is an example log4j2.xml file that sets the root logger level to INFO and outputs log messages to the console:
	<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Root level="INFO">
            <AppenderRef ref="Console"/>
        </Root>
    </Loggers>
</Configuration>

	
	
	2 To configure Log4j using properties in Spring Boot, you can create an application.properties file in the src/main/resources directory. Here is an example application.properties file that sets the root logger level to INFO and outputs log messages to the console:
	
	log4j.rootLogger=INFO, console
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n

	
	
	3 You can also use an application.yml file to specify Log4j properties in YAML format. Here is the equivalent configuration to the above application.properties file, but in YAML format:
	
	log4j:
  rootLogger: INFO, console
  appender:
    console:
      type: consoleAppender
      layout:
        type: patternLayout
        conversionPattern: "%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"

	
	
	
	saving the log file in xml 
	
	<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"/>
        </console>
        <File name="MyFile" fileName="myapp.log"> //just add this .
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"/>
        </File>
    </Appenders>
    <Loggers>
        <Root level="INFO">
            <AppenderRef ref="console"/>
            <AppenderRef ref="MyFile"/>
        </Root>
    </Loggers>
</Configuration>

	
	2 saving the log file , properties file
	
	saving in default path
	log4j.appender.file.File=myapp.log
	
	in desired path
	log4j.appender.file.File=logs/myapp.log
	
	
	
	
	
	
	
