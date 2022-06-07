# WORK IN PROGRESS

# MING (Mosquitto, InfluxDB, NodeRed, Grafana)

MING is a containerised IoT sensor server stack in the traditions of LAMP. This version is specific to Raspberry Pi.

- Mosquitto MQtt broker listening on port 1883 for MQtt message publications

- InfluxDB listening on port 8086 providing a time series database for sensor data storage

- NodeRed listening on port 1880 to provide an easy to use graphical environment for parsing,
  analysing, storing, and forwarding sensor data messages

  The NodeRed InfluxDB nodes are installed by default so you easily store and retrieve data locally.

- Grafana listening on port 3000 providing a data visualisation environment for sensor data.

Each of these applications is built and runs in its own container within Docker

# Supported Targets

Currently tested targets are

- Raspberry Pi 4 64bit

# Getting going

You'll need a Pi and to instal Docker and Docker Compose.

Clone the repo with

`git clone https://github.com/Workshopshed/ming_pi.git`

Build the containers with:

`docker-compose build`

This could take a while on the first run as it has many images to download.
Now run the containers with

`docker-compose up`

Again, on the first run this may take a while as the initial settings are configured and the database installed.

# Connect to the applications

If you Pi hostname is "homedashboard" then you can connect to it using

## Node Red
http://homedashboard.local:1880/

## Grafana
http://homedashboard.local:3000/

## Influx DB
http://homedashboard.local:8086/

# Connecting the applications together

When connecting between the applications from say Grafana to Influx you can refer to the server name as "db" e.g.
http://db:8086 or mqtt://mq:1883 

# Setting Mosquitto Passwords

Start the containers with 

`docker-compose up`

From a separate console connect to the Mosquitto container with

'docker exec -it  ming_pi_mosquitto_1 sh'

Once in use the mosquitto_passwd command to set the passwords https://mosquitto.org/man/mosquitto_passwd-1.html

'mosquitto_passwd /etc/mosquitto/passwd sensor'

Then exit the shell. And restart the containers with

`docker-compose down`
`docker-compose up`

Note that if you change the password in Node-Red there may continue to be errors in the logs until you complete another down/up recycling.

# Volumes

TODO:

So that we don't destroy our SDCard, this project uses an external disk attached to the Pi.
The docker containers should be configured to use this disk.

# Attribution

- This is in part based the work done by the Balena.io team

@see: https://github.com/balena-io-projects/balena-sense

and

- Alex J Lennon (@embedded_iot)
- Julian Todd (@goatchurch)
- Matthew Croughan (@matthewcroughan)

# Contributing

??
