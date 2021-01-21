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
In order to run the parking lot server, run the following command inside the main repository.
```
$ python -m pl_manager.server run
```
## Server explanation
the server defines the app and defines the endpoints it expects. 
        - /park endpoint which expect POST methods to park a car, consisting of a simple JSON that has the car number \n
        to be inserted in the parking lot, for example "{"car-number": 1234567}"
        -/unpark, which expected a json format of the slot to be emptied in the parking lot. for example "{"slot-number":0}"
        -/query/{slot or car-number}/{slot or car-number}, an restful endpoint that expects either /query/slot/0, or /query/car-number/1234567, which returns a Json format of both car number and slot
        number of the relevant car based on what is given.
        brief explanation: the server handles server-related tasks, such as legality of the post or get requests recieved. in addition to that, the app can be created in "debug" mode, in which two new debug endpoints are introduced, which the tests inside the test folder make use of them.
        the server then calls the manager to handle the requests after checking everything is OK, HTTP-Request-wise, and possibly other factos in the future.

# Quickstart Explanation
  The final project includes a client, which streams cognition snapshots to a server, 
  which then publishes them to a message queue, where multiple parsers read the snapshot,
  parse various parts of it, and publish the parsed results, which are then saved to a database.
  The result in the database is then reflected by an API, CLI and GUI.
  
After the snapshot has been updating by the client, A Graphical User Interface (GUI) is available on http://localhost:8080, as well as a Command Line Interface (CLI) by querying like such:
```
$ python -m cortex.cli get-users
[
    {"user_id": 1, "username": "Raghd Zeidan"}
]
$ python -m cortex.cli get-user 1
{
    "user_id": 1, "username": "Raghd Zeidan", "birthday": "1997-10-13", "gender": "m"
}
```
# Full Documentation of Functions and Modules
View the full documenation of modules and functions [here](https://cortexx.readthedocs.io/en/latest/).
  
  
