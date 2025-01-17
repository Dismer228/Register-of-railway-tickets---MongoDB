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
Request Body: 
{
  "startingStopName": "string", 
  "endStopName": "string" 
}

Response: 

• 200 OK: Returns the reservation details. 

    "reservation": { 
        "reservationId": "string", 
        "startingStop": "string", 
        "endStop": "string", 
        "ticketPrice": "float", 
        "routeName": "string", 
        "reservedAt": "datetime" 
    }  
    
• 400 Bad Request: Missing required fields. 

• 404 Not Found: Stops not found or no routes available.
# /buy_ticket (PUT)
Converts a reservation into a purchased ticket. Deletes the reservation after the ticket 
is issued. 

Request Body: 

  { 
    "reservationId": "string", 
    "nameSurname": "string" 
  }

Response: 

• 200 OK: Returns the ticket details. 

    "ticket": { 
        "ticketNumber": "string", 
        "startingStop": "string", 
        "endStop": "string", 
        "ticketPrice": "float", 
        "purchaseTime": "datetime", 
        "routeName": "string", 
        "nameSurname": "string" 
    } 
 
• 400 Bad Request: Missing required fields. 

• 404 Not Found: Reservation not found or expired.
# /ticket/<ticket_number> (GET)
Retrieves the details of a purchased ticket using its ticket number.

Response: 

• 200 OK: Returns the ticket details.
{

    { 
        "ticketNumber": "string", 
        "startingStop": "string", 
        "endStop": "string", 
        "ticketPrice": "float", 
        "purchaseTime": "datetime", 
        "routeName": "string", 
        "nameSurname": "string" 
    } 
}

• 404 Not Found: Ticket not found. 
# /routes (GET)
Retrieves all available routes from the database.
Response: 

• 200 OK: Returns a list of routes. 

[ 

    { 
        "route_id": "int", 
        "name": "string", 
        "stops": [ 
            { 
                "stop_id": "int", 
                "arrival_time": "string", 
                "distance_to_next": "float/null" 
            } 
        ] 
    } 
] 

• 404 Not Found: No routes available.
# /cleanup (POST)
 Deletes all data from the database. This endpoint is for testing purposes.

Response: 
 
• 200 OK: Confirms cleanup completion.

# Notes
1. MongoDB Indexes:

  o PopularRoutesCache: Entries expire after 15 seconds. 
  
  o Reservations: Entries expire after 15 minutes. 
  
  o Tickets: Ensures efficient retrieval of tickets by their unique identifier (ticketNumber) 
  
  o Stops:   stop_id 
      Facilitates fast lookups of stop details based on stop_id. 
      Optimizes route-related operations where stops are queried by their unique ID. 
      
2. Ticket Price Calculation:

  o Direct route: Based on the cumulative distance between stops. 
  
  o Indirect route: Uses an aggregation pipeline to calculate the distance. 
  
  o Formula: 0.14 * distance + 0.3 
  
3. Caching:
   
  o Popular routes and ticket prices are cached in PopularRoutesCache for faster 
    lookups 
