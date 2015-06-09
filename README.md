# CodeSocial API spec
## Intro
CodeSocial exposes a simple stateless JSON REST API to the developer.
Interaction is possible with any regular HTTP client as long as the following
properties of the implementation are kept in mind:

Successful requests return a 200 OK status code.

Failures are limited to:

400 Bad Request.
401 Unauthorized.
403 Forbidden.

All failures will also return an {"error": reason} JSON object.  

All GET routes return valid JSON only.

All POST routes accept and return valid JSON only.

All routes expect the inclusion of a "token" parameter,
use "$pbkdf2-sha512$60000$ZoyySeiSKCXErZNdEn0A8g$Xf0bwmt3avfklCnMSWIGFPSbY0LDrW3N13BB4IW/BmYAoAss/D/ClDGgcTgR96d6NFu2p./QsbEVHoU6dIFnTQ"
for testing/implementation purposes.

POST routes expect the inclusion of an "application/json" "Accept" header.

**CONTINUES SPAMMING OF ROUTES WILL GET YOUR IP BLACKLISTED**

## Endpoints
The API exposes the following endpoints

GET  /api/v1/balance

GET  /api/v1/products

GET  /api/v1/tasks

POST /api/v1/tasks

## GET /api/v1/balance
### Parameters: 
    token={string}
  
### Returns:
    JSON object
    {"balance":  float,
     "currency": string}
  
### Description:
    Returns current, available account balance.
## GET /api/v1/products
### Parameters:
    token={string}

### Returns:
    JSON array
    [{"module": string, 
      "items":  [
        {"category": string, 
          "products": [
            "title": string,
             "price": float, 
             "name": string,
             "id": integer, 
             "description": string
        ]}
    ]}]

### Description:
    Returns a list of available products.
    The root node defines a top-level module (e.g. "instagram") as well as 
    an "items" array associated with the module. 
    Each element of the "items" array represent a "cageory" (e.g. "likes") 
    and a "products" array. Every product includes a "title", a "price" (price per action here),
    a "name", an "id" (used later when POSTing new tasks) and a "description".

## GET /api/v1/tasks
### Parameters:
    token={string}

### Returns:
    JSON object
    {"processing": [
      {"module": string,
       "category": string,
       "processed": integer,
       "to_process": integer,
       "state": string,
       "destination": string, 
       "timestamp": UNIX epoch}
    ], "completed": [
      {"module": string,
       "category": string,
       "processed": integer,
       "to_process": integer,
       "destination": string,
       "timestamp": UNIX epoch}
    ]}

### Description:
    Returns state of tasks that are currently runnign or have finished running ("processed" and "completed" respectively).
    
## POST /api/v1/tasks
### Parameters:
    JSON object
    {"token": string,
     "id": integer, 
     "destination": string, 
     "quantity": integer}

    or 

    JSON object
    {"token": string,
     "id": integer, 
     "destination": array, 
     "quantity": integer}

### Returns:
    JSON object
    {"status": string,
     "currency": string,
     "charge": float}
   
    or 
   
    {"valid_destinations": array,
     "status": "ok",
     "currency": "USD",
     "charge": 7.0}

### Description:
    Creates a new task.
    The "id" parameter specifies the product id retrieved from the /api/v1/products GET route.
    The "quantity" parameter indicates number of desired actions to run on this task (e.g. number of likes to deliver).
    Create a single destination task by passing a "destination" string paramter  (e.g. valid instagram media URL) or a batch task by
    providing multiople destinations in a json array.

    Depending on provided parameters (single or multiple destinations) the response will either come back with a "status": "ok" response
    and contain the total charge (price per action * product price) or contain a "valid_destinations" array and a total charge of 
    price per action * product price * number of valid destinations.