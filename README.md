# OSG-Assignment
Set the mysql database first

mysql -u root -p
create user 'cfbd'@'localhost' identified by '1234';
grant all privileges on *.* to 'cfbd'@'localhost';
create database if not exists user_db;
Initially tables and data will be created automatically by Java Persistence API. New operation can be happened from UI.

Run Eureka naming server to register your services

mvn clean install
mvn spring-boot:run
Go to http://localhost:8761

Check "Instances currently registered with Eureka". Nothing will be shown because you didn't start any services. Its Eureka dashboard, where we can inspecting the registered instances later. In the microservices world, Service Registry and Discovery play role since we most likely run multiple instances of services and we need a mechanism to call other services without hardcoding their hostnames or port numbers. In Cloud environments service instances may come up and go down anytime. So we need some automatic service registration and discovery mechanism. Spring Cloud provides Service Registry and Discovery features, as usual, with multiple options.

Build and run user authentication services

mvn spring-boot:run
It will start on http://localhost:8777//api/auth . To signup -> http://localhost:8777/api/auth/signup .

Refresh Eureka (http://localhost:8761) . Check again "Instances currently registered with Eureka" you will see user-authentication is UP and running. If you stop this services then it will be DOWN.

When Eureka server didn’t get received any notification from a service. Then the service will be unregistered from the Eureka server automatically.

Run Contact service

cd contact-service
mvn spring-boot:run
It will start on http://localhost:8555/ and insert 2 dummy contacts info in contact table with name, contact number and address. Get all Contacts -> http://localhost:8555/contacts/all .

Search by name -> http://localhost:8555/contacts/name/hnjaman here "hnjaman" is the name you searching in your contacts. It will send all contacts containing "hnjaman"

Run sms service

cd sms-service
mvn spring-boot:run
It will start on http://localhost:8999/sms .

To check message for "hnjaman" -> http://localhost:8999/contacts/name/hnjaman/sms . It will get name and contact number from contact service and generate a custom message for "hnjaman" by RESTTemplete. Refresh and check Eureka

Welcome you succesfully started all services use them on Angular UI. Run Angular User Application

cd microservice-ui
Istall node package manager (just for first time)

npm install
ng s --open
It will open automatically in http://localhost:4200/home
