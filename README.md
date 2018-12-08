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

### Example Use Case 

In this example, the API allows clients to access order information using different optimizations based on the client type and requirement. If the client is a mobile application, then filtering can be applied on the response payload to return only certain attributes and not the full content. On the other hand, full response content can be retrieved for third party or internal applicaitons who need to more details.

The following retrieve options are available:

- Filtering fields: using the query parameter "fields", the client app can define which attributes of the order will be returned. This improves network and application performance as only the required attributes are processed.

- Using ETag and If-None-Match headers: the client app can use the values of ETag and If-None-Match to see if updated or new orders have been created from the last request. If the value of ETag is different to a second request, the new orders will be returned, otherwise 304 (Not Modified) with empty body will be returned. This mecanism reduce network bandwith and improve performance making client applications more stable.

- If neither filtering nor headers are passed, the full list will be returned.

- When the client creates an Order, the response will contain a Location header with the identifier of the order. This identifier is required to update or delete an order.

### Set Up and Run the Example 

To run the examples, please follows the below steps:

1. Download and import the Postman collections
1. Creates an Order and keep the value of the Location header.
1. Get Orders: 
  1. Send the request and get the header ETag from the response, 200 OK should be returned.
  1. Set the header If-None-Match with the ETag value and try again, 304 should be returned.
1. Update the Order using the value of the Location header value stored before.
1. Get Orders again, this time a new response will be returned as an order has been updated.
1. Delete an Order using the Location header value.

### References
- [Anypoint Platform](https://anypoint.mulesoft.com/home/#/)
- [Use of If-None-Match and ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-None-Match)
- [Working with partial resources](https://developers.google.com/drive/api/v3/performance).