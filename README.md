# SETUP mosquitto MQTT Broker

### Generate your keys (File needs some adjustments for DOMAIN, FILEPATH)
`./mosquitto/certs/generate-certs.sh`

### Create mosquitto password file
`touch mosquitto/config/mosquitto.passwd`

### Start Docker-Compose
`docker-compose up`

### Change/Set Password for mosquitto
`docker ps
docker exec -it [CONTAINER-ID]  sh`

`#s/> mosquitto_passwd -b [PASSWORDFILE] [USER] [PASSWORD]
#s/> mosquitto_passwd -b mosquitto.passwd phpuser xxxxxxxx
#s/> exit`

`docker-compose down
docker-compose up`


### Structure of MQTT Messages and Paths
```
home-01/groundfloor/frontyard/doorcam/data
{"msgtype":"info","datatype":"jpg","data":"dfkomew0ß2jf2f"}

home-01/groundfloor/garage/wallbox/amp
{"msgtype":"command","datatype":"float","data":10}

home-01/basement/room-south/ecoflow/remaintime
{"msgtype":"info","datatype":"float","data":16.3}

=> the msgtype info - can also be sent directly - without json - just the float value in the payload
home-01/basement/room-south/ecoflow/remaintime
16.3
```


### .env File
```
# Config File for Mosquitto MQTT Application
# Port Configuration Mosquitto MQTT
MQTT_PORT=1883
MQTT_TLS_PORT=8883

# Timezone
TZ=Europe/Berlin

#INFLUXDB config
INFLUXDB_INIT_MODE=setup
INFLUXDB_INIT_USERNAME=admin
INFLUXDB_INIT_PASSWORD=xxxxxxxxxxxxxxxxxxxxxx
INFLUXDB_INIT_ORG=fmi
INFLUXDB_INIT_BUCKET=fmihome

#GoLang MQTT-InfluxDB Sync config
INFLUXDB_BUCKET_TOKEN="xxxxxxxxxxxxxxxxxxxxxxxxx"
INFLUXBD_URL="http://influxdb-server:8086"
INFLUXBD_ORG=fmi
INFLUXBD_BUCKET=fmihome
MQTT_BROKER_URL="tcp://mqtt-server:1883"
SUBSCRIBE_TO_TOPIC="home-01/#"
MQTT_USER=user
MQTT_PASSWORD=xxxxxx
MQTT_CLIENT_ID=golang-sync-client

#Postgres-Server
POSTGRES_USER=postgres
POSTGRES_PASSWORD=xxxxxxxxxx

#Postgres homepanda
POSTGRES_HT_HOST=localhost
POSTGRES_HT_USER=homepanda
POSTGRES_HT_PASSWORD=xxxxxxxxxxxxxxx
POSTGRES_HT_DBNAME=homepanda
```
