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

## Main class of the connector
The main class of the connector is the /ericsson/appiot class, from which you obtain references to other classes:
```

var appIoT = new 

## resolutions
Resolutions are used to aggregate data based on custom-defined time spans.
