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

Note that the platform specific options are at the top of each Dockerfile e.g. --platform=linux/arm64v8

# Getting going

You'll need a Pi and to install Docker and Docker Compose.

Clone the repo with

`git clone https://github.com/Workshopshed/ming_pi.git`

Build the containers with:

`docker-compose build`

This could take a while on the first run as it has many images to download.
Now run the containers with

`docker-compose up`

Again, on the first run this may take a while as the initial settings are configured and the database installed.

# Connect to the applications

If your Pi hostname is "homedashboard" then you can connect to it using

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

The containers create a user "sensor" with the password "changeme" you can change this with the following process.

Start the containers with 

`docker-compose up`

From a separate console connect to the Mosquitto container with

`docker exec -it  ming_pi_mosquitto_1 sh`

Once in use the mosquitto_passwd command to set the passwords https://mosquitto.org/man/mosquitto_passwd-1.html

`mosquitto_passwd /etc/mosquitto/passwd sensor`

Then exit the shell. And restart the containers with

`docker-compose down; docker-compose up`

Note that if you change the password in Node-Red there may continue to be errors in the logs until you complete another down/up recycling.

# Volumes

So that we don't destroy our SDCard, this project uses an external disk attached to the Pi.

The docker-compose file is configured with a datadir environment variable which is in turn defined in the .env file.
The default location is /media/datadrive which is where you'd mount your external drive.

## Rootless Docker

If you are running rootless mode docker https://docs.docker.com/engine/security/rootless/ then you will need to ensure that the permissions are configured on your external drive to allow the remapped userids to modify their own files. This can be done by granting permission to the root of the data drive and then creating the folders for each of the containers to store their data.

```
chmod 777 /media/datadrive/
mkdir /media/datadrive/mosquitto-data
mkdir /media/datadrive/influxdb-data
mkdir /media/datadrive/nodered-data
mkdir /media/datadrive/grafana-data
```

# Attribution

- This is in part based the work done by the Balena.io team

@see: https://github.com/balena-io-projects/balena-sense

and

- Alex J Lennon (@embedded_iot)
- Julian Todd (@goatchurch)
- Matthew Croughan (@matthewcroughan)

# Contributing

Not sure if I'll be actively maintaining this project but feel free to clone it and ask questions.
