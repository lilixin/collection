###Maven使用教程
####Maven项目默认约定
![maven](image/maven.jpg)
编译后 的 classes 会放在 basedir/target/classes 下面， JAR 文件会放在 ${basedir}/target 

###Maven安装与配置
1. 下载maven安装包 [apache-maven-3.2.1](http://download.csdn.net/detail/u013142781/9355367)
2. maven环境变量配置 MAVEN_HOME PATH `%MAVEN_HOME%\bin;`
3. 设置apache-maven-3.2.1\conf\setting.xml文件 配置本地仓库 localRepository的值设置成你本地仓库的路径

###eclipse mavenr插件配置
1. 下载[eclipse-maven3-plugin](http://download.csdn.net/detail/u013142781/9355661)
2. 插件安装完成后 Windows–>Prefrences–>Installations–>Add。installation name选maven的根目录

###Maven的常用命令
    1. 创建Maven的普通java项目： 
    mvn archetype:create -DgroupId=packageName -DartifactId=projectName 
    2. 创建Maven的Web项目： 
    mvn archetype:create -DgroupId=packageName -DartifactId=webappName-DarchetypeArtifactId=maven-archetype-webapp 
    3. 编译源代码： mvn compile 
    4. 编译测试代码：mvn test-compile 
    5. 运行测试：mvn test 
    6. 产生site：mvn site 
    7. 打包：mvn package 
    8. 在本地Repository中安装jar：mvn install 
    9. 清除产生的项目：mvn clean 
    10. 生成eclipse项目：mvn eclipse:eclipse 
    11. 生成idea项目：mvn idea:idea 
    12. 组合使用goal命令，如只打包不测试：mvn -Dtest package 
    13. 编译测试的内容：mvn test-compile 
    14. 只打jar包: mvn jar:jar 
    15. 只测试而不编译，也不测试编译：mvn test -skipping compile -skipping test-compile 
    ( -skipping 的灵活运用，当然也可以用于其他组合命令) 
    16. 清除eclipse的一些系统设置:mvn eclipse:clean
###Maven工程创建
![mave1](image/maven1.jpg)
![mave2](image/maven2.jpg)  

######输出项目名，包（Packaging，如果只是普通的项目，选jar就好了，如果是web项目就选war，这里我们选择jar）  

![mave3](image/maven3.jpg)
![mave4](image/maven4.jpg)

######现在我们添加mysql驱动包的依赖,编辑pom.xml如下：
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <groupId>com.luo</groupId>
      <artifactId>first_maven_project</artifactId>
      <version>0.0.1-SNAPSHOT</version>
    
      <dependencies>
    <!-- mysql驱动包 -->
    <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.29</version>
    </dependency>
    </dependencies>    
    </project>

######对应依赖的包的dependency 可以在这个网站找[http://mvnrepository.com/](http://mvnrepository.com/)
************
************
************
###配置远程仓库
####在POM中配置远程仓库
这种方式只能在本项目中使用(不推荐)

####在settings.xml中配置远程仓库(推荐)
	<！--这里配置公司的nexus私服-->
    <profile>
    	  <id>nexus</id>
      <repositories>
    <repository>
      <id>nexus</id>
      <url>http://192.168.1.5:8081/nexus/content/repositories/public</url>
    		  <snapshots> 
    			<enabled>true</enabled> 
    		  </snapshots> 
    		  <releases> 
    		 	<enabled>true</enabled> 
    		  </releases>		  
    </repository>
      </repositories>
      <pluginRepositories>
      	<pluginRepository> 
      		<id>nexus</id> 
      		<url>http://192.168.1.5:8081/nexus/content/repositories/public</url>
      		<snapshots>
      			<enabled>false</enabled>
      		</snapshots>
    		</pluginRepository>
      	<pluginRepository> 
      		<id>central</id> 
      		<url>http://repo1.maven.org/maven2</url>
      		<snapshots>
      			<enabled>false</enabled>
      		</snapshots>
    		</pluginRepository>		
      </pluginRepositories>
    </profile>
    	<profile>
    	   <id>sonar</id>
    	   <activation>
    		   <activeByDefault>true</activeByDefault>
    	   </activation>
    	   <properties>  
    		   <sonar.jdbc.url>
    			 jdbc:mysql://192.168.1.7:3306/sonar
    		   </sonar.jdbc.url>
    		   <sonar.jdbc.driver>com.mysql.jdbc.Driver</sonar.jdbc.driver>
    		   <sonar.jdbc.username>user</sonar.jdbc.username>
    		   <sonar.jdbc.password>user</sonar.jdbc.password>  
    		   <sonar.host.url>http://192.168.1.7:9080/sonar/</sonar.host.url>
    	   </properties>
    	</profile>	
    	
    	<!-- 设置mavn创建项目时默认的jdk版本 -->
    	<profile>  
    		<id>jdk-1.8</id>  
    		<activation>  
    			<activeByDefault>true</activeByDefault>  
    			<jdk>1.8</jdk>  
    		</activation>  
    		<properties>  
    			<maven.compiler.source>1.8</maven.compiler.source>  
    			<maven.compiler.target>1.8</maven.compiler.target>  
    			<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  
    		</properties>  
    	</profile> 
    
      </profiles>
 
######这里配置国内速度比较快的镜像        
    <mirrors>
    <mirror>  
      <id>repo2</id>  
      <mirrorOf>central</mirrorOf>  
      <name>Human Readable Name for this Mirror.</name>  
      <url>http://repo2.maven.org/maven2/</url>
    </mirror>
	<mirror>  
            <id>maven.mirrorid</id>  
            <mirrorOf>central</mirrorOf>  
            <name>Human Readable Name for this Mirror.</name>  
            <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>  
        </mirror>  
        <mirror>  
            <id>mirrorId</id>  
            <mirrorOf>*</mirrorOf>  
            <name>Human Readable Name for this Mirror.</name>  
            <url>http://maven.mirrorid/content/groups/public/</url>  
        </mirror>  
    </mirrors>
************
************
************

###指定Maven分发构件的位置(pom.xml)
mvn deploy 用来将项目生成的构件分发到远程Maven仓库

	<distributionManagement>
		<repository>
			<id>releases</id>
			<name>Internal Releases</name>
			<url>http://192.168.1.5:8081/nexus/content/repositories/releases</url>
		</repository>
		<snapshotRepository>
			<id>snapshots</id>
			<name>Nexus Snapshot Repository</name>
			<url>http://192.168.1.5:8081/nexus/content/repositories/snapshots</url>
		</snapshotRepository>
	</distributionManagement>
######分发构件到远程仓库需要认证

     <server>
       <id>releases</id>
       <username>deployment</username>
       <password>deployment</password>
     </server>
    	 <server>
       <id>snapshots</id>
       <username>deployment</username>
       <password>deployment</password>
     </server>
      </servers>

###通用打包配置(pom.xml)
	<build>
		<resources>
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.xml</include>
				</includes>
			</resource>
		</resources>
		<pluginManagement>
			<plugins>
				<!-- resource插件, 设定编码 -->
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-resources-plugin</artifactId>
					<version>2.7</version>
					<configuration>
						<encoding>${project.build.sourceEncoding}</encoding>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-source-plugin</artifactId>
					<version>2.4</version>
					<configuration>
						<encoding>${project.build.sourceEncoding}</encoding>
					</configuration>
					<executions>
						<execution>
							<id>attach-sources</id>
							<goals>
								<goal>jar</goal>
							</goals>
						</execution>
					</executions>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-javadoc-plugin</artifactId>
					<version>2.10.1</version>
					<executions>
						<execution>
							<id>attach-javadocs</id>
							<goals>
								<goal>jar</goal>
							</goals>
						</execution>
					</executions>
				</plugin>
				<!-- compiler插件, 设定JDK版本及编码 -->
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-compiler-plugin</artifactId>
					<version>3.2</version>
					<configuration>
						<source>${jdk.version}</source>
						<target>${jdk.version}</target>
						<encoding>${project.build.sourceEncoding}</encoding>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-jar-plugin</artifactId>
					<version>2.5</version>
					<configuration>
						<archive>
							<addMavenDescriptor>false</addMavenDescriptor>
						</archive>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-war-plugin</artifactId>
					<version>2.5</version>
					<configuration>
						<resourceEncoding>${project.build.sourceEncoding}</resourceEncoding>
						<archive>
							<addMavenDescriptor>false</addMavenDescriptor>
						</archive>
					</configuration>
				</plugin>

				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-surefire-plugin</artifactId>
					<version>2.12.4</version>
					<configuration>
						<skip>true</skip>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-checkstyle-plugin</artifactId>
					<version>2.13</version>
					<configuration>
						<configLocation>config/maven_checks.xml</configLocation>
					</configuration>
				</plugin>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-jxr-plugin</artifactId>
					<version>2.5</version>
				</plugin>
			</plugins>
		</pluginManagement>
		<plugins>
			<plugin>
			    <groupId>org.apache.maven.plugins</groupId>
			    <artifactId>maven-source-plugin</artifactId>
			    <executions>
			        <execution>
			            <id>attach-sources</id>
			            <goals>
			                <goal>jar-no-fork</goal>
			            </goals>
			        </execution>
			    </executions>
			</plugin>
		</plugins>
	</build>



