# ecrisson
This connector wraps the client and admin API exposed by Ericsson's AppIoT platfrom, to which the connector gives you access through JavaScript objects and methods. 

The AppIoT platform provides IP based southbound (downstream) and northbound (upstream) communication with gateways and devices and offers the following main features:
- device management
- data management (storing raw data and calculating derived data)
- creating rules and triggers (invocation of callbacks)

# How to use the connector

## Configuration
The /ericsson/config script contains variables you need to update with your own values, mainly:

- token should be set to a developer authentication token obtained from Ericsson
- adminToken should be set to an admin authentication token obtained from Ericsson
- appIoTPrefix should point to your App IoT endpoint. Unless otherwise specified by Ericsson, keep the default value

To obtain tokens (developer and administrator), sign in to the AppIoT platform and [click on Settings > API Keys](https://eappiot.sensbysigma.com/#/apikeys/apikeys)]

Also note that you can also provide had hoc configuration when instanciating the AppIoT class (see below)

## AppIoT: the main class of the connector
The main class of the connector is the /ericsson/appiot class, from which you obtain references to other classes:

```
var appIoTModule = require("/modules/ericsson/appiot");
// instanciation using config file 
var appIoT = new appIoTModule.AppIoT(); 
// instanciation with ad hoc config
var adhocConfig = {
  token: "some_dev_token",
  adminToken: "some_admin_token"
};

var adhocAppIoT = new appIoTModule.AppIoT(adhocConfig); 
```
## Sensors
If you need to manipulate sensors defined in AppIoT, you can use the SensorManager and Sensor classes. 
You can obtain an instance of the SensorManager from the AppIoT instance or directly by instanciating the class, as illustrated in the below code snippet. The first option is preferrable as much easier, unless you need some specific customization.

```
// Get an instance of SensorManager from the AppIoT instance
var sensorManager = appIoT.getSensorManager();  

// Create an instance of SensorManager using its constructor
var sensormanagerModule = require("/ericsson/clientapi/sensorManager");
var clientModule = require("/ericsson/client");
var client = new clientModule.Client({});
var sensorManager = new sensormanagerModule.SensorManager({"client": client});
```
Using the SensorManager, you can list all sensors within a collection
```
// list all sensors in collection and pick on sensor from the list
var list = sensorManager.listSensorsInCollection({collectionId: "some_collection_id"});
// list is an array of Sensor instanced
```
Using the SensorManager you can also get a specific sensor instance by id
```
var sensor = sensorManager.getSensor({Id: sensorId});
```
Using the Sensor class instances, you can obtain the list of measures made by the device or get a specific the latest value measured by the device
```
var measureList = sensor.listMeasurements(); // all measures
var measure = sensor.getMeasurement(); // latest measure
```

## resolutions
Resolutions are used to aggregate data based on custom-defined time spans. You manipulate Resolutions using the ResolutionManager and Resolution classes. Obtaining an instance of ResolutionManager is easily done using the AppIoT class but can also be done through direct instanciation.

```
// Get an instance of ResolutionManager from the AppIoT instance
var resolutionManager = appIoT.getResolutionManager();  

// Create an instance of ResolutionManager using its constructor
var resolution = require("/ericsson/clientapi/resolutionManager");
var clientModule = require("/ericsson/client");
var client = new clientModule.Client(); 
var resolutionManager = new resolution.ResolutionManager({"client": client});
``` 
Using the ResolutionManager, you can list available Resolutions 
```
// list all resolutions
var list = resolutionManager.listResolutions();
// list is an array of Resolution instances
```
You can also get a specific Resolution by id
```
// get a specific resolution
var resolution = resolutionManager.getResolution({id : resolutionId});
```
Using an instance of Resolution, you can list all measures made by a specific device, filtering by start/end data en number of data points
```
var measures = resolution.listMeasurements({
        "sensorId" :"some_id",
        "startTime":"some_ISO_date",
        "endTime":"some_ISO_date",
        "maximumNumberOfDataPoints":some_max_number
});
```
