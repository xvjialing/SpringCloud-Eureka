# 在DockerCompose环境下实现Eureka服务的注册与发现

* 构建镜像

  在根目录下执行以后命令构建镜像

  ```shell
  $ mvn clean package -Dmaven.test.skip=true

  [INFO] Scanning for projects...
  [INFO] ------------------------------------------------------------------------
  [INFO] Reactor Build Order:
  [INFO]
  [INFO] spring-cloud
  [INFO] eureka-client
  [INFO] eureka-server
  [INFO]
  [INFO] ------------------------------------------------------------------------
  [INFO] Building spring-cloud 0.1.0-SNAPSHOT
  [INFO] ------------------------------------------------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ spring-cloud ---
  [INFO]
  [INFO] ------------------------------------------------------------------------
  [INFO] Building eureka-client 0.0.1-SNAPSHOT
  [INFO] ------------------------------------------------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ eureka-client ---
  [INFO]
  [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ eureka-client ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] Copying 1 resource
  [INFO] Copying 1 resource
  [INFO]
  [INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ eureka-client ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to /Users/xvjialing/IdeaProjects/SpringCloud/eureka-client/target/classes
  [INFO]
  [INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ eureka-client ---
  [INFO] Not copying test resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ eureka-client ---
  [INFO] Not compiling test sources
  [INFO]
  [INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ eureka-client ---
  [INFO] Tests are skipped.
  [INFO]
  [INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ eureka-client ---
  [INFO] Building jar: /Users/xvjialing/IdeaProjects/SpringCloud/eureka-client/target/eureka-client-0.0.1-SNAPSHOT.jar
  [INFO]
  [INFO] --- spring-boot-maven-plugin:1.5.9.RELEASE:repackage (default) @ eureka-client ---
  [INFO]
  [INFO] --- docker-maven-plugin:0.4.13:build (default) @ eureka-client ---
  [INFO] Copying /Users/xvjialing/IdeaProjects/SpringCloud/eureka-client/target/eureka-client-0.0.1-SNAPSHOT.jar -> /Users/xvjialing/IdeaProjects/SpringCloud/eureka-client/target/docker/eureka-client-0.0.1-SNAPSHOT.jar
  [INFO] Copying /Users/xvjialing/IdeaProjects/SpringCloud/eureka-client/src/main/docker/Dockerfile -> /Users/xvjialing/IdeaProjects/SpringCloud/eureka-client/target/docker/Dockerfile
  [INFO] Building image xvjialing/eureka-client
  Step 1/5 : FROM openjdk:8-jre-alpine

  Pulling from library/openjdk
  Digest: sha256:8d6f291d02454a43c312b771915c24c1abcf95324de579945a4ac86e4561ac2a
  Status: Downloaded newer image for openjdk:8-jre-alpine
   ---> b1bd879ca9b3
  Step 2/5 : VOLUME /tmp

   ---> Using cache
   ---> 91ddca9b507f
  Step 3/5 : ADD eureka-client-0.0.1-SNAPSHOT.jar app.jar

   ---> 0ee66a2b6ef9
  Step 4/5 : EXPOSE 8081

   ---> Running in 86270e0ea6fe
  Removing intermediate container 86270e0ea6fe
   ---> f8fc8f62bfc6
  Step 5/5 : ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]

   ---> Running in b14c07178431
  Removing intermediate container b14c07178431
   ---> 15050400530f
  ProgressMessage{id=null, status=null, stream=null, error=null, progress=null, progressDetail=null}
  Successfully built 15050400530f
  Successfully tagged xvjialing/eureka-client:latest
  [INFO] Built xvjialing/eureka-client
  [INFO]
  [INFO] ------------------------------------------------------------------------
  [INFO] Building eureka-server 0.0.1-SNAPSHOT
  [INFO] ------------------------------------------------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ eureka-server ---
  [INFO]
  [INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ eureka-server ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] Copying 1 resource
  [INFO] Copying 1 resource
  [INFO]
  [INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ eureka-server ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to /Users/xvjialing/IdeaProjects/SpringCloud/eureka-server/target/classes
  [INFO]
  [INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ eureka-server ---
  [INFO] Not copying test resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ eureka-server ---
  [INFO] Not compiling test sources
  [INFO]
  [INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ eureka-server ---
  [INFO] Tests are skipped.
  [INFO]
  [INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ eureka-server ---
  [INFO] Building jar: /Users/xvjialing/IdeaProjects/SpringCloud/eureka-server/target/eureka-server-0.0.1-SNAPSHOT.jar
  [INFO]
  [INFO] --- spring-boot-maven-plugin:1.5.9.RELEASE:repackage (default) @ eureka-server ---
  [INFO]
  [INFO] --- docker-maven-plugin:0.4.13:build (default) @ eureka-server ---
  [INFO] Copying /Users/xvjialing/IdeaProjects/SpringCloud/eureka-server/target/eureka-server-0.0.1-SNAPSHOT.jar -> /Users/xvjialing/IdeaProjects/SpringCloud/eureka-server/target/docker/eureka-server-0.0.1-SNAPSHOT.jar
  [INFO] Copying /Users/xvjialing/IdeaProjects/SpringCloud/eureka-server/src/main/docker/Dockerfile -> /Users/xvjialing/IdeaProjects/SpringCloud/eureka-server/target/docker/Dockerfile
  [INFO] Building image xvjialing/eureka-server
  Step 1/5 : FROM openjdk:8-jre-alpine

   ---> b1bd879ca9b3
  Step 2/5 : VOLUME /tmp

   ---> Using cache
   ---> 91ddca9b507f
  Step 3/5 : ADD eureka-server-0.0.1-SNAPSHOT.jar app.jar

   ---> f894e0fae17e
  Step 4/5 : EXPOSE 8761

   ---> Running in 633b1dff4438
  Removing intermediate container 633b1dff4438
   ---> 2e396e000bcc
  Step 5/5 : ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]

   ---> Running in 71293772cca7
  Removing intermediate container 71293772cca7
   ---> 11d803652b46
  ProgressMessage{id=null, status=null, stream=null, error=null, progress=null, progressDetail=null}
  Successfully built 11d803652b46
  Successfully tagged xvjialing/eureka-server:latest
  [INFO] Built xvjialing/eureka-server
  [INFO] ------------------------------------------------------------------------
  [INFO] Reactor Summary:
  [INFO]
  [INFO] spring-cloud ....................................... SUCCESS [  0.127 s]
  [INFO] eureka-client ...................................... SUCCESS [  9.488 s]
  [INFO] eureka-server ...................................... SUCCESS [  4.401 s]
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time: 14.716 s
  [INFO] Finished at: 2018-05-04T15:11:32+08:00
  [INFO] Final Memory: 61M/521M
  [INFO] ------------------------------------------------------------------------
  ```

* 以docker-compose的形式运行Spring Cloud服务发现注册服务

  ```shell
  # 先切换到docker目录下
  $ cd docker 
  # 以docker-compose的方式运行程序
  $ docker-compose up -d
  ```

  ​