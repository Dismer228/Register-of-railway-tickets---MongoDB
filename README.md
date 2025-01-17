# Register-of-railway-tickets---MongoDB
The service can register train routes. Each route has a list of stops. The DB was set up using Docker. The applications was made using python and needs pymongo, bson, datetime, uuid and flask libraries.
Includes these methods:
# API Endpoints
# /init (POST)
Initializes the database with predefined stops, routes, and indexes. Ensures 
collections are properly populated if empty.
Response: 
• 200 OK: Returns a message confirming initialization.
# /preview_ticket (POST)
Calculates the ticket price and reserves a ticket for a specific route between two 
stops.
• Request Body: 
{ 
  "startingStopName": "string", 
  "endStopName": "string" 
} 

Response: 
• 200 OK: Returns the reservation details. 
{ 
    "reservation": { 
        "reservationId": "string", 
        "startingStop": "string", 
        "endStop": "string", 
        "ticketPrice": "float", 
        "routeName": "string", 
        "reservedAt": "datetime" 
    } 
} 
• 400 Bad Request: Missing required fields. 
• 404 Not Found: Stops not found or no routes available.
