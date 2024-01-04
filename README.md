# Introduction
As part of the Distributed Systems class, a project was developed to implement a management platform for an electric scooter fleet in the form of a client-server pair in Java using sockets and threads. The service's goal is to allow users to reserve and park scooters at different locations. The map is a grid of N x N locations, in this case, a 20 x 20 grid, with geographical coordinates represented as discrete even indices. The distance between two locations is calculated using the Manhattan distance.

To ensure a good distribution of scooters across the map, there's a rewards system where users are rewarded for moving scooters parked at one location A to another location B. Location A should have more than one available scooter, and there should be no scooter within a radius D=2 at location B. In this case, the group chose to reward users with a 20% discount on the trip price. Each reward is identified by an origin-destination pair.

This service also provides a notification system that can be activated and deactivated by users. They can receive notifications when there are rewards within a fixed distance of D=2 from a specific location.
# Platform structure
## Server
The server initializes a socket on port 1100, ensuring the exchange of messages with the client. This socket is known to all clients. Therefore, when a connection is established, whenever a client makes a request, it will be responded to by the server. Additionally, the server utilizes methods and data stored in memory in the "App" class to fulfill the requests made by the users.
## App
The "App" class serves as the storage for the map containing the scooters, organized in a matrix. This class contains various essential methods used by the server to execute the functionalities present in the program.
## Client
The client connects to the server through the socket created with port 1100. In this program, all clients are required to have an associated account. Therefore, when this connection is established, a main menu is presented to the client. Here, the client can either log in, register, or exit the program. If the client chooses one of the first two options and successfully completes the action, they are directed to a second menu where they have access to various functionalities within the program. Each of these options in these menus leads to the creation of an execution thread.
## Client -> Server comunication
The client needs to input certain required data to execute most of the functionalities within the program. These data are sent to the server through the "send" method created in the "Demultiplexer" class, which, in turn, calls the method with the same name in the "TaggedConnection" class. To differentiate the type of message being sent, the data in the socket are forwarded along with a tag. Each tag corresponds to a different functionality, enabling the server to distinguish between the types of messages being received.
## Server -> Client comunication
The server, in order to send the results obtained from executing the functionalities, uses the "send" method from the "TaggedConnection" class, which writes the tag and the data onto the socket. The "start" method of the "Demultiplexer" class organizes the responses received by the client, storing all the received data based on the tag in a buffer, forming a waiting queue. This method executes the thread responsible for interrupting all messages sent by the server to the socket. As a result, the "receive" method of the "Demultiplexer" class is invoked when there is a client thread waiting to receive the data. When this queue is empty, the thread waits for data to arrive using the "await" method.
# Functionalities
**Authentication and Registration**: Users need to authenticate or register with their username and password whenever they wish to interact with the service. The methods trataFazerLogin() and trataRegistar() in the TextUI class handle this functionality by verifying the client's input on the server.

**Listing Locations with Available Scooters**: The trotinetes_livres(int x, int y) method in the App class takes coordinates inserted by the client and returns a list of locations with available scooters within a fixed distance of D=2 from the specified location.

**Listing Rewards within a Fixed Distance**: The recompensas_na_areas(int X, int Y) method in the App class lists rewards with origins within a fixed distance of D=2 from a location inserted by the client.

**Reserving a Scooter**: The reserva_trotinete(int x, int y) method in the App class reserves a free scooter closest to a location specified by the client, limited to a distance of D=2.

**Parking a Scooter**: Parking a scooter by providing the reservation code and location is done using the reserva_trotinete method in the class. The server informs the client about the trip cost, whether it is a normal trip or a rewarded one, with the assistance of the clientes method in the Servidor class.

**Notification System**: Clients can request to be notified or turn off notifications when rewards with origins within a fixed distance of D=2 from a particular location appear. This functionality is handled by the clientes method in the Servidor class, with the help of the espera_notificações method in the same class.
