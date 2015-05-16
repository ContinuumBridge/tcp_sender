# tcp_sender
An app that listens on a TCP socket and sends all data it receives from adaptors to it.

The app will forward from devices that are connected to it that have the following characteristics:

* acceleration
* gyro
* temperature
* ir_temperature
* magnetometer
* buttons
* humidity
* binary_sensor
* power
* battery
* connected
* luminance

The characterisitcs that are forwarded can be controlled using a file /opt/cbridge/thisbridge/tcp_sender.config. The defaults are:

    {
      "temperature": "True",
      "temp_min_change": 0.1,
      "irtemperature": "False",
      "irtemp_min_change": 0.5,
      "humidity": "True",
      "humidity_min_change": 0.2,
      "buttons": "False",
      "accel": "False",
      "accel_min_change": 0.02,
      "accel_polling_interval": 3.0,
      "gyro": "False",
      "gyro_min_change": 0.5,
      "gyro_polling_interval": 3.0,
      "magnet": "False",
      "magnet_min_change": 1.5,
      "magnet_polling_interval": 3.0,
      "binary": "True",
      "luminance": "True",
      "luminance_min_change": 1.0,
      "power": "True",
      "power_min_change": 1.0,
      "battery": "True",
      "battery_min_change": 1.0,
      "connected": "True",
      "slow_polling_interval": 600.0,
      "TCPport": 5003
    }

You only need to specify the things that you want to change. The "min_change" keys specify the minumum amount a characteristic needs to change by before it is forwarded.

If the example above, add the app to a bridge and connect some devices to it. Restart the bridge, and after things have got going, type the following in another shell on the same computer:

    nc localhost 5003
    
This starts netcat in client mode listening to port 5003. Here's an example of the output to expect:

    {"m": "data", "d": {"points": [[1431677071671, 6718]], "name": "BID144/Fibar/luminance"}}
    {"m": "data", "d": {"points": [[1431677087409, 100]], "name": "BID144/Fibar/battery"}}
    {"m": "data", "d": {"points": [[1431677097848, 24.1]], "name": "BID144/Fibar/temperature"}}
    {"m": "data", "d": {"points": [[1431677220568, 1]], "name": "BID144/Fibar/binary"}}
    {"m": "data", "d": {"points": [[1431677223701, 2709]], "name": "BID144/Fibar/luminance"}}
    {"m": "data", "d": {"points": [[1431677226824, 1361]], "name": "BID144/Fibar/luminance"}}
    {"m": "data", "d": {"points": [[1431677230997, 0]], "name": "BID144/Fibar/binary"}}
   
As you can see, this is in JSON format and can be de-serialised in any application program. The format is actually one that can be used directly with InfluxDB's API. The points consist of an epoch time in ms since the epoch (not seconds) and a value. 
