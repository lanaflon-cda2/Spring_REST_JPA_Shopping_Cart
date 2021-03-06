# Spring_REST_JPA_Shopping_Cart
A sample spring boot REST API app that enables CRUD operations on Products. Users can place Orders containing multiple such products. The price of products can be changed but already placed orders will still be able to show the old prices used during order placement. Spring JPA and H2 in memory DB are used for this app.

## Build project

This code required the following to be built/packaged.

### Required Softwares
Maven 3.0+<br>
Java 1.8<br>
[H2 database and Tomcat included in the code with spring boot dependencies]

### Steps
1. Run the below maven command to install the app.This creates an executable jar file in /target folder.

**mvn install**

2. To run the app using java command line, run the following command.

**java -jar demo-0.0.1-SNAPSHOT.jar**

### Swagger Endpoint
http://localhost:8080/swagger-ui.html#/

### H2 DB console
http://localhost:8080/console/

Driver Class: org.h2.Driver<br>
JDBC URL: jdbc:h2:mem:testdb<br>
username: sa<br>
password: <none><br>



## API Description

### 1. Products
These endpoints allow for CRUD operation on the products repository.

#### a. Create a product
This request creates a product and auto generates an id. The response will be HTTP 200 for success and a json containing the created product is returned

##### Request
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"name":"paper","price":1}' 'http://localhost:8080/products/'

##### Response
{
  "id": 1,
  "name": "paper",
  "price": 1
}


#### b. Read a product
There are two options to read products.One endpoint to read a prticular product and another to list all available products.

##### Request all products
curl -X GET --header 'Accept: application/json' 'http://localhost:8080/products/'

##### Response
[
  {
    "id": 1,
    "name": "paper",
    "price": 1.2
  }
]

##### Request a particular product
curl -X GET --header 'Accept: application/json' 'http://localhost:8080/products/1'

##### Response
{
  "id": 1,
  "name": "paper",
  "price": 1.2
}


#### c. Update a product
This request updates an existing product. The response will be HTTP 200 for success and a json containing the updated product is returned
In this example we are updating the price of the product with id 1

##### Request
curl -X PUT --header 'Content-Type: application/json' --header 'Accept: application/json' -d **'{"name":"paper","price":1.2}' 'http://localhost:8080/products/1'**

##### Response
{
  "id": 1,
  "name": "paper",
  "price": 1.2
}


#### d. Delete a product
This request removes a product from the database. The response will be HTTP 200 for success.

##### Request
curl -X DELETE --header 'Accept: */*' 'http://localhost:8080/products/1'

##### Response Code
200

### 2. Orders
These endpoints allow for creating,reading,deleting and searching through orders based on start and end dates.

#### a. Create an order
This request creates an order and auto generates an id. The response will be HTTP 200 for success and a json containing the created product is returned.
**The main prerequisite is to already have the products available and then reference the product ids in the order JSON under the shoppingCartItemsList.**

##### Request
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ "email":"abc%40def.com", "paymentSuccessFul":true, "shoppingCartItemsList":[ { "quantity":5, "product":{ "id":2 } },{ "quantity":2, "product":{ "id":3 } } ] }' 'http://localhost:8080/orders/'

##### Response
{
  "id": 2,
  "email": "abc@def.com",
  "orderTotalPrice": 8,
  "paymentSuccessFul": true,
  "orderDate": "2018-12-06T16:15:59.313+0000",
  "shoppingCartItemsList": [
    {
      "quantity": 5,
      "price": 1,
      "product": {
        "id": 2,
        "name": "PAPER",
        "price": 1
      }
    },
    {
      "quantity": 2,
      "price": 1.5,
      "product": {
        "id": 3,
        "name": "PEN",
        "price": 1.5
      }
    }
  ]
}


#### b. Read an order
There are three options to read orders.
- Read all orders
- Read a particular order with order id
- Search for orders within a timeframe.

Let's see the search request here:

##### Search for Orders within a time frame
curl -X GET --header 'Accept: application/json' 'http://localhost:8080/orders/search?startdate=2018-12-06T14%3A38%3A22.236Z&enddate=2018-12-06T17%3A00%3A50.936Z'

##### Response
[
  {
    "id": 2,
    "email": "abc@def.com",
    "orderTotalPrice": 8,
    "paymentSuccessFul": true,
    "orderDate": "2018-12-06T16:15:59.313+0000",
    "shoppingCartItemsList": [
      {
        "quantity": 5,
        "price": 1,
        "product": {
          "id": 2,
          "name": "PAPER",
          "price": 1
        }
      },
      {
        "quantity": 2,
        "price": 1.5,
        "product": {
          "id": 3,
          "name": "PEN",
          "price": 1.5
        }
      }
    ]
  }
]


#### c. Delete an order
This request removes the order from the database.

##### Request
curl -X DELETE --header 'Accept: */*' 'http://localhost:8080/orders/1'

##### Response Code
200
