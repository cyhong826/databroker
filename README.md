# Data Broker

It is a data broker that allows exchanging data between distributed modules. 
Modules are distributed entities able to exchange data and keep state synchnization between them. 
This broker is target to applications where data must be shared among modules 
and there must exist state consistence between them. 
Applications such as distributed simulation, simulators, multi-agent systems and multiplayer game may apply for use this broker.

# How does it work ?

Modules provide and consume data periodically using the data connection interface.
The broker builds data routes using the module configuration file, which describes the data needed by each module. 
The same principle is used in the Control communication channel, used to forward command packets between modules. 
Commands are a special kind of data, which are not produced periodically and may be used to module synchronization, 
module configuration or to delivery low rate data.

# Configuration 

## Modules Configuration

Example of Module configuration file.

   ```
{
   "id": "radio",
   "name":"Radio Communication",
   "description": "This module receives and send data throught RF to the robots",
   "ip": "127.0.0.1",
   "port_data":6002,
   "port_control":6003,
   "socket_type":"udp",

   "data_published": [ 
      {
         "id": "vx0",
         "name": "Estimated position X",
         "description": "Measured Velocity X from robot 0",
         "unit": "m/s"
      },
      {
         "id": "vy0",
         "name": "measured velocity Y",
         "description": "Measeured velocity Y from Robot 0",
         "unit": "m/s"
      },
      {
         "id": "ball_sensor",
         "name": "Ball Pocession Sensor ",
         "description": "Indicates if the robot got the ball procession",
         "unit": "True of False"
      }
   ],

   "data_consumed": [ 
      {
         "source": "ai",
         "id": "vx0" 
      },
      {
         "source": "ai",
         "id": "vy0" 
      }
   ],

   "commands_consumed": [ 
      {
         "source": "ai",
         "command": "kick"
         }
      ]	
}

   ```

Following you can see the desciption of each parameter.


| Parameter              | Description           | Data Type  | Optional |
| ---------------------- |:---------------------:| :--------: | :-------: | 
|   **id**               |  Module Identification name. It must be unique. | *String*  - Unique | **No** |
|   **name**             |  Module name in human readable format. |  *String*| **yes** |
|   **description**      |  Module description in human readble format.|  *String* | **Yes** |
|   **port_data**        |  Socket port for the Data Channel|  *Unsigned Integer 16-bits* | **No** |
|   **port_control**     |  Socket port number for the Control Channel | *Unsigned Integer 16-bits*  | **No** | 
|   **socket_type**      |  Socket type for the Data Channel. Two values possible *"tcp"* or *"udp"* | **String** - "tcp" or "udp" | **Yes**: Default is "udp" | 
|   **data_published**   |  List of data descriptor objects. Data descrptors describes each data provided by the module.| JSON object list. | **Yes** |
|   **data_consumed**    |  List of data consumed by the module.| *JSON object list* | **Yes** |
|   **commands_consumed**|  List of commands consumed by the module| *JSON Object list* | **Yes**|

### Data Published

| Parameter              | Description           | Data Type  | Optional |
| ---------------------- |:---------------------:| :--------: | :-------: |
|**id**| Data id. must be unique in the given module.|*String*|**No**|
|**name**| Data name in human readable format.|*String*|**Yes**|
|**description**| Data description in human readble format.|*String*|**Yes**|
|**unit**| Data Unit. It may be a pysical unit, such as m, m/s, degrees or something that represent the unit. |*String*|**Yes**|

### Data Consumed

| Parameter              | Description           | Data Type  | Optional |
| ---------------------- |:---------------------:| :--------: | :-------: |
|**source**| Module Id of the data provider. |*String*|**No**|
|**id**| Data Id |*String*|**No**|

### Commands consumed

| Parameter              | Description           | Data Type  | Optional |
| ---------------------- |:---------------------:| :--------: | :-------: |
|**source**| Module Id of the command provider|*String*|**No**|
|**command**| Command string |*String*|**No**|

When no command consumed is provided, the module that is sending the command must specify 
the destination in the command packet. The explicit definition in-packet of the destinations has 
precedence over the pre-defined route table. More information can be found in the section *Communication API*.

## Broker Configuration

Example of broker configuration file. Modules are a list of 
data paths to the module configuration files. The Path is relative 
to the config.json file.

   ```
{
   "data_rate": 0,
   "modules": [
      "vision.json",
      "radio.json"
      ]
}
   ```

| Parameter              | Description           | Data Type  | Optional |
| ---------------------- |:---------------------:| :--------: | :-------: |
|**data_rate**| Minimum time between data packets in milliseconds (ms) |*Unsigned Integer* | **Yes** - default is 0|
|**modules**| Path to the module descriptors. Paths are relative to the broker configuration directory. | *String* | **No** |

## Communication API


# Dependencies
 - [Qt >= 5.4](https://www.qt.io/)
 - [Google Protobuffer](https://developers.google.com/protocol-buffers/)
