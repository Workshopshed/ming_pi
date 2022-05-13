# WORK IN PROGRESS

# MING (Mosquitto, InfluxDB, NodeRed, Grafana)

MING is a containerised IoT sensor server stack in the traditions of LAMP. This version is specific to Raspberry Pi.

- Mosquitto MQtt broker listening on port 1883 for MQtt message publications

- InfluxDB listening on port 8086 providing a time series database for sensor data storage

- NodeRed listening on port 1880 to provide an easy to use graphical environment for parsing,
  analysing, storing, and forwarding sensor data messages

  We've also installed the NodeRed InfluxDB nodes by default so you easily store and retrieve
  data locally.

- Grafana listening on port 80 providing a data visualisation environment for sensor data.

Each of these applications is built and runs in its own container within Docker

# Supported Targets

Currently tested targets are

- Raspberry Pi 4

# Getting going

You'll need a Pi and to instal Docker and Docker Compose.

# Volumes

So that we don't destroy our SDCard, this project uses an external disk attached to the Pi.
The docker containers are configured to use this disk.

# Attribution

- This is in part based the work done by the Balena.io team

@see: https://github.com/balena-io-projects/balena-sense

and

- Alex J Lennon (@embedded_iot)
- Julian Todd (@goatchurch)
- Matthew Croughan (@matthewcroughan)

# Contributing

??
