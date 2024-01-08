# Electric scooter fleet
As part of the Distributed Systems class, a project was developed to implement a management platform for an electric scooter fleet in the form of a client-server pair in Java using sockets and threads. The service's goal is to allow users to reserve and park scooters at different locations. The map is a grid of N x N locations, in this case, a 20 x 20 grid, with geographical coordinates represented as discrete even indices. The distance between two locations is calculated using the Manhattan distance.

To ensure a good distribution of scooters across the map, there's a rewards system where users are rewarded for moving scooters parked at one location A to another location B. This service also provides a notification system that can be activated and deactivated by users.

For all this to be possible, we implemented a server, an APP and a client programs. Whenever a client makes a request, it will be responded to by the server, wich utilizes methods and data stored in memory in the APP. 

