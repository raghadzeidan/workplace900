# Parking Lot Assignment

# Installation
in order to install the requirements, run the following command inside the main repository.
```
$ . install.sh
```

Run tests to make sure everything is working ok
```
$ pytest tests/
```

# The server
In order to run the parking lot server with the default parameters, run the following command inside the main repository.
```
$ python -m pl_manager.server run
```

In order to create the app with default parameters, use ```create_app()``` function inside ```server.py```
In order to run the app after it was created, use ```run_server(app,host,port)```
## Server Parameters
### run_server() parameters
*app* parameter in which app to run, most likely the app created by create_app() function
*host* default parameter being **localhost**. \
*port* default parameter being default flask port **5000**. 
### create_app() parameters
create_app() functions takes takes 4 parameters.
*parking_lot_cls* defines the parking-lot class the app is initiated with. This defines the class that indicates the state of the parking lot we are creating an app for. For our assignment, this state is simply just an integer indicating the number of slots in the parking lot. the create_app function then creates an instance of this class, that takes into account the parking lot state (in our case, the create_app function just retreives the SLOTS_NUMBER environment variable and initiates the parking lot via it.) \
The default value for the *parking_lot_cls* parameter is ```SimpleParkingLot``` class defined in ```pl_manager.py``` that expected a single number_of_slots integer to initiate it.
*backend_driver_cls* defines the driver used in order to save/retreive data from our database. In the assignment case, the driver I implemented was ```SimpleJsonDriver``` inside ```json_driver.py```. it writes to "db.json" file in the db directory, a json-format of the databse, including a "free" key that is mapped to the number of free slots in our database, as well as "slots" key that is mapped to a slot-number,car-number set of pairs (dict).

*db_path* is the path to the database that it used by our backened_driver, **if it uses one**. the default value of it, **for our assignment purposes** is the path to the "db.json" file that holds the ```SimpleJsonDriver``` database.
*debug* indicates wether or not we're creating the app in debug-mode or not. debug-mode includes more endopints that are utilized by our tests. default value is **False**

## Breif Server/Assignment Explanation
The solution and the parameters for the create_app() function, define 3/4 SW "layers". \
1) The server: the server handles server-related tasks, such as legality of the post or get requests recieved, as well as defining limits for traffic, and possibly other factors in the future. In addition to that, its responsible of "combining" the parameters explained above and defining a **manager** object which is of class ```PL_Manager``` \

2) The manager (```PL_Manager```): the manager class is responsible for managing the requests that need to be processed, it takes two instances to initialize, one is the Parking Lot instance, and the other is Database driver instance. Its job is to query the parking lot instance for the legality of the given parameters (for example, that the car number is in fact a 7-digit number), before triggering the database driver to save it in the DB. this class could be further extended in the future, if for example there is some sort of metadata that needs to be processed and saved before giving it to the database driver. This is one step "below" the server layer, as it doesn't deal with legality of requests, but instead manages the need-to-process informations received and passed on by the server. \

3) The parking lot (```SimpleParkingLot```): This defines the state of the parking lot. In our assignment, the state is only an integer that indicates the number of slots available in the parking lot, but again, it could be extended to a more complex state that require more processing. this "devision" between them allows us to introduce more complex parking lots, and connect it to the manager and introduce a new parking lot to the system (with ofcourse, introducing a new db_driver that takes into account the complexity of the parking lot, the parking lot and the database driver are very connected to each other, yet they're seperated and composed in order to ease adaptability of the system to changes). \

4) The database driver (```SimpleJsonDriver```): This is the last layer of the of the system, in which the driver simply updates/retrieves data from the database. everything passed to it has been checked for legality by the server and the parking lot manager (with help of simpleparkinglot that define the "rules" of the parking lot), so the SimpleJsonDriver just simply stores and retreives without taking anything else into account other than Json-formatting the DB and stuff.. \

**ASSIGNMENT Note:** 4+) ```SimpleStaticDriver``` I introduced this driver at the end of my solution, in order to check how well the system deals with changes in the database and drivers, and I indeed only needed to implement the relvenat database driver functions, such as add_car, empty_slot and retreival functions that the manager (1 layer above) uses. it also connected very well with the tests written, I only had to change the parameter of the create_app function to indicate that we're using a SimpleStaticDriver instance rather than SimpleJsonDriver, and it allowed me to test it completely independent from the rest of the system. Check git logs for commits with prefix "ASSIGNMENT: " for further explanation.


## Server Endpoints
the server defines the app and defines the endpoints it expects. \
        - /park endpoint which expect POST methods to park a car, consisting of a simple JSON that has the car number 
        to be inserted in the parking lot, for example "{"car-number": 1234567}". \
        run this command in order to send the relevant post-request for adding a car. note that it sends to the default parameters of the server. \
        ```
        $ python -m pl_manager.server add-car 1234567
        ``` \
        - /unpark, which expected a json format of the slot to be emptied in the parking lot. for example "{"slot-number":0}". \
        run this command in order to send the relevant post-request for emptying a parking slot. note that it sends to the default parameters of the server.\
        ```
        $ python -m pl_manager.server empty-slot 0
        ``` \
        - /query/{slot or car-number}/{slot or car-number}, an restful endpoint that expects either /query/slot/{slot_number} or /query/car-number/{car_number} GET requests, which returns a Json format of both car number and slot number of the relevant car based on what is given. \
        for example if car with number **1234567** sits in parking slot **0**, then get-request ```/query/slot/0``` or ```/query/car-number/1234567```in order to retreive information about the car. \
        return value for the above requests will be ```{"slot-number":0, "car-number":1234567}``` Json format.
        
        
## Quickstart explanation
        brief explanation: the server handles server-related tasks, such as legality of the post or get requests recieved. in addition to that, the app can be             created in "debug" mode, in which two new debug endpoints are introduced, which the tests inside the test folder make use of them.
        the server then calls the manager to handle the requests after checking everything is OK, HTTP-Request-wise, and possibly other factos in the future.

# Quickstart Explanation
  The final project includes a client, which streams cognition snapshots to a server, 
  which then publishes them to a message queue, where multiple parsers read the snapshot,
  parse various parts of it, and publish the parsed results, which are then saved to a database.
  The result in the database is then reflected by an API, CLI and GUI.
  
After the snapshot has been updating by the client, A Graphical User Interface (GUI) is available on http://localhost:8080, as well as a Command Line Interface (CLI) by querying like such:
# Full Documentation of Functions and Modules
View the full documenation of modules and functions [here](https://cortexx.readthedocs.io/en/latest/).
  
  
