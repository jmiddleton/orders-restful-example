# Order Management 

This REST API manages order purchases. The client can retrieve orders, create, update or cancel a specific order. The API is secured using OAuth 2.0.

To access these endpoints the client application needs an API key which is provided by the server application hosting this API.

The API exposes the following operations:

- List orders
- Create an order
- Update an order
- Deletes an order 

### Mulesoft Application

The application was created using the Design Center and it is hosted on [Anypoint Platform](https://anypoint.mulesoft.com/home/). 

To access the API use this URL http://order-backend-jipu.us-e2.cloudhub.io.
The following methods are allowed:

- GET     /v1/order-management/orders: retrieves all the orders persisted.
- POST    /v1/order-management/orders: create an order.
- PUT     /v1/order-management/orders/{orderId}: update an order.
- DELETE  /v1/order-management/orders/{orderId}: delete an order.

The application uses MongoDB to store/retrieve/update/delete orders. The database is hosted on [mLab](https://mlab.com/). Before using the application, please configure the attribute connectionString of mongo:connection-string-connection.

### Example Use Case 

In this exercise, the API allows clients to access order information using different optimizations based on the client type and requirement. If the client is a mobile application, then filtering can be applied on the response payload to return only certain attributes and not the full content. On the other hand, full response content can be retrieved for third party or internal applications who need to more details.

During retrieve, the following options are available:

- Filtering fields: using the query parameter "fields", the client app can define which attributes of the order will be returned. This improves network and application performance as only the required attributes are processed.

- ETag and If-None-Match headers: the client app can use the values of ETag and If-None-Match to see if updated or new orders have been created from the last request. If the value of ETag is different to a second request, the new orders will be returned, otherwise 304 (Not Modified) with empty body will be returned. This mechanism reduces network bandwidth and improve performance making client applications more stable.

When the client creates an Order, the response will contain a Location header with the identifier of the order. This identifier is required to update or delete an order.

### Set Up and Run the Example 

To test how caching might be used by a third party consumer, execute the following steps:

1. Download and import the Postman collections
1. Open the sample request "Create Order" and change some attribues of the body. After execution, please grab the value of the Location header.
1. Open "Get Orders - ETag" and execute the request. Get the value of ETag header. 200 OK should be returned.
1. Using the ETag value, set the If-None-Match header try again. This time, 304 should be returned.
1. Open the test "Update Order" and set the value {orderId} in the path with the value of the Location header returned by the "Create Order" test. Send the request and wait for 200 OK.
1. Again execute "Get Orders - ETag", this time a new response will be returned as an order has changed.
1. Open the test "Delete Order" and using the Location header from the "Create Order" test, delete the order.

To test how a mobile application can limit the amount of information returned, do the following:

1. Open the test "Get Orders - Mobile" and execute it.
1. Now change or add an attribute i.e.: links, shippingAddress, deliveryDate, currency, totalBeforeTax, etc. and execute it again. The expected response should return only the attributes you have defined in the "fields" query parameter.

### References
- [Anypoint Platform](https://anypoint.mulesoft.com/home/#/)
- [Use of If-None-Match and ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-None-Match)
- [Working with partial resources](https://developers.google.com/drive/api/v3/performance).
