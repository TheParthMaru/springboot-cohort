# What is MVC architecture?

- Stands for Models, Views and Controllers.
- Restaurant example
- Advantage: Separation of concerns
- Waiter = Controller
- Chef (has the business logic) = Model
- Menu (mostly frontend) = View
- The client server architecture doesn't talk about how the server processes the request.
- We need a mechanism to explain the internal processing and one of the way is MVC architecture.

## Difference between Controllers and Models

![alt text](/images/controller-vs-model.png)

## Problem with MVC architecture

- It is a superficial architecture.
- Chef seems to be loaded with lot of responsibilities apart from preparing the meal.
- And apart from the chef & waiter, who is managing the customers, who is managing the inventory of food supplies and other required resources.
- Here, model is also interacting and transacting data with the database and third-party APIs.
- The separation of concern is not achieved in the best possible way.
- Hence, this model is an over-simplified.

## Realistic industry-level architecture

![alt text](/images/different-mvc-layers.png)

- In actual projects, the controller and models are divided into more smaller layers with smaller responsibilties to achieve true separation of concern.

1. **Routing layer:** Identifies which controller should be triggered for the incoming request. For small frameworks like express, we need to explicitly code the routes. But for large frameworks like Springboot, the routing is handled by the Dispatcher Servlet and we don't have to worry much about it. So the routing layer collects the request and forwards it to the required handler or controller for further processing.

2. **Handlers/Controllers:** Responsible for forwarding the incoming request to required business logic (service layer), wait for the processing to be done and then carry the response back. Apart from this, controller is also responsible for preparing the response object (json response or any other form).

3. **Service layer:** Responsible for the business logic. It is not responsible to communicate with DB or third-party API. The reason is that if any change in the data store system, a lot of code needs refactoring in the service layer. Apart from service layer, the tests for the service layer also needs refactoring. So service layer is solely for business logic. DB and third-party communications are handled in another layer which helps us achieve separation of concern.

4. **Repository layer:** Query the DB. Service layer calls the relevant methods of the repository layer in order to get the data. Service layer is not concerned on how the data is being fetched. That is handled in the repository layer. For example, if currently the data is being fetched from MySQL DB and we want to fetch it from MongoDB instead of MySQL, we just need to modify the repository layer for this particular problem. Service layer needs the data to be served and is not bothered if it is being called from MongoDB or MySQL. We use repository pattern for the repository layer.

5. **API/ Gateway layer:** Responsible for the third-party API calls.

6. **Data Transfer Object (DTO) layer:** This layer is responsible for the structure of the request and response that needs to be transferred within the application or through the external API. The structure of data that needs to be transferred internally and externally is decided in this layer. The object of this layer is widely used in the controller layer. It doesn't represent how the data is stored in the DB.

7. **Entity layer:** Exact representation of how the data is in the database.

8. **Validation layer:** Used to validate the request or the data of the request. Widely used for email validation, data types validation in forms, etc.
