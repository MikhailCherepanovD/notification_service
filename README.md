# Flight ticket tracking application

## Motivation

I often fly on airplanes so that I know in advance about which dates I want to fly (for example, from January 27 to January 30). I don't care about the exact date of the trip, and I want to save money and fly cheaper. 

The prices of plane tickets depend on many factors: the current demand for tickets, the date of the trip, airline promotions, the phases of the moon, the constellations of Oreon and etc.  Therefore, the best option to buy a cheap ticket is to monitor prices in real time and buy when they drop! It is even more convenient to receive notifications about the current minimum ticket prices on set dates in your messenger.

Try application: <http://84.201.170.212:8082/>
## Functionality

The service allows you to

* Set one or more routes for tracking on certain dates and get the current price in the web application;
* Receive notifications by mail and telegram on specified routes at selected time intervals;
* View price statistics for the specified routes in the web application;

## Architecture

[](architecture_scheme.jpg)

## Сomponents

* **Notification_Service:** Microservice that implements the basic logic of the application:

    - User management (CRUD operations);  
    - Route management (CRUD operations);  
    - Integration with the Aviasales API to retrieve route information; 
    
    Writen on C++ Dragon Framework. 
    
    In more details: <https://github.com/MikhailCherepanovD/notification_service_drogon>
    
* **Web_Page_Manager:**  Microservice for web pages managment and sending queries to Notification_Service, a layer between the web client and the main backend server. User session management is implemented here.

    Writen on Python Flask Framework. 

    In more details: <https://github.com/MikhailCherepanovD/notification_service_flask>


* **Messange_Sending_Service:**  Microservice that that reads information about tracked tickets from a dedicated view and sends it to the message broker Apache Kafka at specified time intervals. 

    Writen on C++ Dragon Framework. 
    
    In more details: <https://github.com/MikhailCherepanovD/sending_messages_notification_service>

* **Email_Message_Sirvice**  and **Telegram_Message_Sirvice:** Microservices that is listen Apache Kafka and send messages to email and telegram. Telegram_Message_Sirvice  is connected to the redis key-value storage for storing client chat IDs.

    Writen on Python Flask Framework. 

    In more details: <https://github.com/MikhailCherepanovD/notification_email_sender>

    And: <https://github.com/MikhailCherepanovD/notification_telegram_sender>


* **DB:** Postgresql database. It contains 6 regular tables, a trigger, a table for the trigger, and a view. Some data processing functions are also written here.

    In more details: <https://github.com/MikhailCherepanovD/notification_email_sender>



 The logic written in Web_Page_Manager can be transferred to the frontend or  to the Notification_Service, but I was interested in implementing the RESTfull API for the interaction of two backend services.

 ## Deploy

Each of the microservices is assembled in a docker container and managed via docker-compose. The application is deployed and running on the Yandex Cloud VM.

Link to deploy repository: <https://github.com/MikhailCherepanovD/notification_service_deploy>

Link to my Dockerhub: <https://hub.docker.com/u/cherepmd>

### To run

    git clone https://github.com/MikhailCherepanovD/notification_service_deploy

    docker-compose up -d
