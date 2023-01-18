Docker Commands
https://levelup.gitconnected.com/dockerizing-spring-boot-mysql-application-73e09a485c0a
—-------------------------------------
Applications properties:
spring.datasource.url=jdbc:mysql://mysqldb:3306/employeedb?useSSL=false
#spring.datasource.url=jdbc:mysql://<container-name>:<container-external-port>/<dbname>?useSSL=false
spring.datasource.username=sa
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

Build java App:
docker build -t backend .

Create docker network
docker network create springmysql-net

Network list:
docker network ls

MYSQL:
docker run -p 3306:3306 --name mysqldb --network springmysql-net -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=employeedb -e MYSQL_USER=sa -e MYSQL_PASSWORD=1234 -d mysql:5.7

Spring APP:
docker run -p 8080:8080 --name employee-app -itd --network=springmysql-net backend
(In detached mode)

Logs:
docker logs -f 62f65e547d89

Stop docker Apps:
docker stop $(docker ps -aq)

Remove docker containers:
docker rm $(docker ps -aq)
http://localhost:8080/api/employees

http://localhost:8080/swagger-ui.html#!/employee45controller/getEmployeesUsingGET