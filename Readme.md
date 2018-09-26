# Jersey without Maven

This is a toy project that contains the necessary libs to run jersey + tomcat 9 under java 10 with no maven dependencies.

Doing so is clearly a bad idea in production.

There is a good chance that this project will not work well on some OSes.


## Info :

Here is how i managed to get the libraries :


I generated a project using this :
```bash
mvn archetype:generate -DarchetypeGroupId=org.glassfish.jersey.archetypes     -DarchetypeArtifactId=jersey-quickstart-webapp -DarchetypeVersion=2.26
```

Then i modified the pom.xml to this :

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

	<modelVersion>4.0.0</modelVersion>

	<groupId>fr.istic.rest</groupId>
	<artifactId>test</artifactId>
	<packaging>war</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>test</name>

	<build>
		<finalName>test</finalName>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.5.1</version>
				<inherited>true</inherited>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
				</configuration>
			</plugin>
			<plugin>
				<artifactId>maven-dependency-plugin</artifactId>
				<executions>
					<execution>
						<phase>install</phase>
						<goals>
							<goal>copy-dependencies</goal>
						</goals>
						<configuration>
							<outputDirectory>${project.build.directory}/lib</outputDirectory>
						</configuration>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.glassfish.jersey</groupId>
				<artifactId>jersey-bom</artifactId>
				<version>${jersey.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<dependencies>
		<dependency>
			<groupId>org.glassfish.jersey.containers</groupId>
			<artifactId>jersey-container-servlet-core</artifactId>
			<!-- use the following artifactId if you don't need servlet 2.x compatibility -->
			<!-- artifactId>jersey-container-servlet</artifactId -->
		</dependency>

		<dependency>
			<groupId>org.glassfish.jersey.media</groupId>
			<artifactId>jersey-media-json-binding</artifactId>
		</dependency>

		<dependency>
			<groupId>javax.activation</groupId>
			<artifactId>activation</artifactId>
			<version>1.1.1</version>
		</dependency>
		<!-- <dependency> <groupId>org.glassfish.jersey.media</groupId> <artifactId>jersey-media-moxy</artifactId> 
			</dependency> -->
		<dependency>
			<groupId>org.glassfish.jersey.inject</groupId>
			<artifactId>jersey-hk2</artifactId>
		</dependency>
		<dependency>
			<groupId>javax.xml.bind</groupId>
			<artifactId>jaxb-api</artifactId>
			<version>2.3.0</version>
		</dependency>

	</dependencies>
	<properties>
		<jersey.version>2.27</jersey.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>
</project>

```


Then i ran the following command :
```bash
mvn clean install
```

All maven dependencies are then copied to target/lib
