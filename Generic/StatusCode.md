* 200 OK 
* 201 Created - Created
* 204 No Content - Success but no response body.

* 3xx — Redirection

* 400 Bad Request
* 401 Unauthorized
* 403 Forbidden - Identity known but Permission denied
* 404 Not Found
* 409 Conflict [ request is valid but conflict with current stored value]
* 422 Unprocessable Entity - business rules fail ho gaye [ age: -5]

* 500 Internal Server Error - application level error like db connection.
* 502 Bad Gateway - Nginx found no node server running 
* 503 Service Unavailable - node server alive but temporarily unable to serve requests
