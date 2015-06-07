# cs_api_spec
# Intro
CodeSocial exposes a simple stateless JSON REST API to the developer.
Interaction is possible with any regular HTTP client as long as the following
properties of the implementation are kept in mind:

Successful requests return a 200 OK status code.

Failures are limited to:
  
400 Bad Request 
401 Unauthorized
403 Forbidden

All failures will also return an {"error": reason} JSON object.  

All GET routes return valid JSON only.

All POST routes accept and return valid JSON only.

All routes expect the inclusion of a "token" parameter,
use "$pbkdf2-sha512$60000$ZoyySeiSKCXErZNdEn0A8g$Xf0bwmt3avfklCnMSWIGFPSbY0LDrW3N13BB4IW/BmYAoAss/D/ClDGgcTgR96d6NFu2p./QsbEVHoU6dIFnTQ"
for testing/implementation purposes.

POST routes expect the inclusion of a "application/json" "Accept" header.

**CONTINUES SPAMMING OF ROUTES WILL GET YOUR IP BLACKLISTED**

# Endpoints
The API exposes the following endpoints
GET  /api/v1/balance
GET  /api/v1/products
GET  /api/v1/tasks
POST /api/v1/tasks
