# Solar Village
This project is developed using Process Automation Engine 7 (PAM7) where we can send and process the order request, get the approval from government Agencies and complete the request.
## Main features:
-	Process the new order request
-	Get Approval from HOA (using human task i.e.,we can claim the task by using an admin role)
-	After approval,parallel process will happen to send and persists permit requests(webservices developed using spring-boot.The webservice process also contains the service to check the database for any of the pending task and approve that.)
-	If approved,the process will be completed.
-	If denied,the rollback of data will happen.
## Prereqs:
-	Java
-	Maven
-	PAM7 with kie server
-	Oracle database
-	Springboot project(GovernmentPermit)
## Installation:
-	Install PAM7 and kie server ( I have a JBPM)
-	Configure database with following details(for springboot project)(change the configuration as you need in application-properties)

* **url**     :  jdbc:oracle:thin:@localhost:1521:xe
* **username**: test_database
* **password**: asdf123

-	clone the repo for SolarVillage Project and clone the repo for [GovernmentPermit](https://github.com/SaranyaDevi4/GovermentPermit).
-	Build and deploy the SolarVillage project with kie-container creation)
-	Build the Government Permit project and start using
-	Java –jar GovernmentPermit.jar  (or) mvn spring-boot:run
## Test the project:
-	If you use the container name with the same name of the project GAV, (i.e SolarVillage_3.0.13) you will be able to see in Business Central, all the process instances and tasks created created by Kie Server.
## Kie Server REST API:
-	Headers to use with POST and PUT (Auth required, you can use i.e JBPM user): Accept:application/json Content-Type:application/json

- **To Start Process Instance:(will get processinstance id): (POST method)**

http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/processes/SolarVillage.neworderpermitting/instances

{
  "newOrder": {
    "com.myspace.solarvillage.NewOrder": {
      "id": 2,
      "name": "saranya",
      "address": "M Street Demo",
      "isAssociatedWithHOA": true     
    }
  }
}

- **List available tasks for potential owners : (here we can see the tasks with task id and the owners and groups)**

http://localhost:8080//kie-server/services/rest/server/queries/tasks/instances/pot-owners
- **Salesman approval from sales department group to accept the HOA meeting**

 **Claim task  with taskinstance id for own user ( I am JBPM user) (PUT method)**
 
http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/tasks/{taskinstanceid}/states/claimed?user=jbpm

- **Start task  with taskinstance id for own user ( I am JBPM user)(PUT method)**

http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/tasks/{taskinstanceid}/states/started?user=jbpm

- **Complete task  with taskinstance id for own user ( I am JBPM user)(PUT Method)**

http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/tasks/{taskinstanceid}/states/completed?user=jbpm


{
  "newOrder": {
    "com.myspace.solarvillage.NewOrder": {
      "id": 2,
      "name": "saranya",
      "address": "M Street Demo",
      "isAssociatedWithHOA": true,
      "isMeetingScheduled": true
    }
  }
}
- **After 30 S,HOA meeting will happen.here,the task will be waiting to get approval from HOA group
So repeat the steps to claim and start the process as a jbpm user)**

-	http://localhost:8080//kie-server/services/rest/server/queries/tasks/instances/pot-owners

-	http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/tasks/{taskinstanceid}/states/claimed?user=jbpm

-	http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/tasks/{taskinstanceid}/states/started?user=jbpm

-	http://localhost:8080/kie-server/services/rest/server/containers/SolarVillage_3.0.13/tasks/{taskinstanceid}/states/completed?user=jbpm

{
  "newOrder": {
    "com.myspace.solarvillage.NewOrder": {
      "id": 2,
      "name": "saranya",
      "address": "M Street Demo",
      "isAssociatedWithHOA": true,
      "isMeetingScheduled": true,
"isApprovedFromHOA":true
    }
  }
}

 
