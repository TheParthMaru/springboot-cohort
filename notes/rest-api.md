# REST APIs

## API

- Stands for Application Programming Interface.
- APIs are basically functionalities of any application that are exposed to the outside world in an abstracted way.
- Client will request the servers to access this functionalities through an API.
- The API acts as an intermediary, processing the requests and returning responses.
- API can be called by a network request or a function call.

### COWIN API Example

- During COVID, the Government of India had created an API called "COWIN API".
- This API was used by Paytm, Telegram bots, etc to notify users for the availability of vaccines.

![alt text](/images/COWIN%20API.png)

- How the internals of booking an appointment works is not exposed.

- APIs are piece of code that helps to exposed the internal business functionality in an abstracted way.

### UPI Example

- UPI is a payment mechanism that works in India.
- Many clients such as GPay, PhonePe, Paytm, CRED-UPI helps to initiate UPI payments.
- These apps are TPAPs (Third Party Applications).
- But none of these apps have contributed a single line of code to the business logic of UPI.
- The business logic of UPI is written by NPCI and resides on the NPCI servers.
- The APIs to initiate a UPI transaction is exposed abstractly which is used by the payment TPAPs.
- The business logic and the transaction takes place on the NPCI servers once the correct UPI payment request is initialised.
- NPCI doesn't have different business logic for different TPAPs but a unified logic which can be exposed using an API.

### Zomato Food Delivery App

![alt text](/images/zomato.png)

- Zomato has a common backend which exposes all the relevant APIs.
- The Android, iOS and Web Apps are the clients that initiates network requests to the Zomato's common backend.

## REST

- Representational State Transfer.
- Set of recommendations to prepare modern web APIs.
- Its still okay to not follow the recommendations.
- If we follow the REST API conventions then our APIs become more concrete, consistent, readable and standard that a lot of developers use.
- The API contract becomes more consistent.
- It is protocol independent (HTTP/ HTTPS/ etc).
- It is implementation independent.
- Designing of the API is more important.
- Apart from REST, we also have SOAP, gRPC, etc.

### Why REST?

- REST is the most widely accepted API standard.
- Better community support.
- Frameworks like Ruby on Rails, Django, etc are heavily tied with REST.
- Understanding REST is far easy.

### Principles of REST

**1. All the APIs should be designed around resources.**

- Resource is any kind of data, object, service or real life entity that a client can access.
- For BookMyShow, `movie`, `show`, `user`, etc is a resource.
- Syntax: `IPAddress:PORT/ROUTES`
- Example: `https://localhost:3000/movie/3`
- Your route should be based on resources.
- Example: `/products`, `/customers`, `/users`, `/orders`, `/categories`
- In gRPC, the API should be designed around action.
- So, if movie is a resouce, booking a movie is an action.

**2. Use JSON format to send and receive data.**

```json
{
	"name": "parth",
	"age": 26,
	"isEmployed": true
}
```

## Ways to send data from client or server

- There are three ways to do this:

**1. Query params**

- `/route/?key1=value1&key2=value2&key3=value3`
- On the server side, this data is collected and segregated in the form of key-value pair.
- After the `?` everything is treated as a query params in the form of key-value pairs separated by an `&`.
- Widely used for filter based request.
- Not a good use case for requesting sensitive information like username and password.

**2. URL params**

- Also called path params.
- Example: `/todo/23`.
- Here, we are referring to the 23rd todo in our todo list.
- It's not key-value pair.
- `/todo/{:id}` where `id` can change.

**3. Body params**

- The HTTP request object contains body params.
- We can send some data in body params.
- Used to send complex nested data.

## HTTP Methods

- `GET`
- `POST`
- `PUT`
- `PATCH`
- `DELETE`

## CRUD APIs

- Any resource can be created, read, updated and deleted.

1. Create product API

- `/product` -> `POST`
- Send the product details as body param.

2. Get all products

- `/products` -> `GET`

3. Get a product

- `/products/:product-id` -> `GET`

4. Delete a product

- `/products/:product-id` -> `DELETE`

## HTTP Response Status Codes

- HTTP response status codes indicate whether a specific HTTP request has been successfully completed.
- They are grouped as follows:

  1. Informational responses (100 - 199)
  2. Successfull responses (200 - 299)
  3. Redirection messages (300 - 399)
  4. Client error responses (400 - 499)
  5. Server error responses (500 - 599)

## URI vs URL vs URN

- Lets understand these terms with reference to an E-Commerce project.

| **Aspect**                   | **URI**                                                                                                     | **URL (subset of URI)**                                                                 | **URN (subset of URI)**                                                          |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Meaning**                  | Generic identifier for a resource (can be a name, a location, or both).                                     | Identifier that **tells you where and how** to access a resource.                       | Identifier that gives a **unique name**, but not the location or access method.  |
| **Analogy**                  | “Something that points to a product.”                                                                       | “The product’s full web address you can visit.”                                         | “The product’s catalog number / SKU.”                                            |
| **E-commerce Example**       | Any of these could be a URI:<br>• `https://amazon.com/product/12345` (location)<br>• `urn:sku:12345` (name) | `https://www.amazon.com/dp/B0CXYZ1234?ref=abc`<br>(takes you to the exact product page) | `urn:asin:B0CXYZ1234`<br>(Amazon Standard Identification Number for the product) |
| **Tells you how to access?** | Not always                                                                                                  | Yes (protocol like `https`, domain, path)                                               | No (just a name, no access method)                                               |
| **Used in real life for**    | Any resource reference                                                                                      | Visiting product pages, APIs, images, checkout links                                    | Internal product catalogs, ISBNs, SKUs, ASINs                                    |

## More REST Recommendations

## Things to explore

- More of REST recommendations
- URL encoding and decoding
- When to use query params, body params and URL params.
- What is request body?

## Important Resource And Documentations

- [Official MDN Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- [RESTful web API design by Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [REST API Design Guidance by Microsoft](https://microsoft.github.io/code-with-engineering-playbook/design/design-patterns/rest-api-design-guidance/)
