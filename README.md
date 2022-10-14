# SETUP mosquitto MQTT Broker

### Generate your keys (File needs some adjustments for DOMAIN, FILEPATH)
`./mosquitto/certs/generate-certs.sh`

```
#!/bin/bash

#Create this script (generate-certs.sh) - change IP Adress and generate keys

IP="192.168.1.22"
SUBJECT_CA="/C=SE/ST=Stockholm/L=Stockholm/O=himinds/OU=CA/CN=$IP"
SUBJECT_SERVER="/C=SE/ST=Stockholm/L=Stockholm/O=himinds/OU=Server/CN=$IP"
SUBJECT_CLIENT="/C=SE/ST=Stockholm/L=Stockholm/O=himinds/OU=Client/CN=$IP"

functiongenerate_CA() {
   echo"$SUBJECT_CA"
   openssl req -x509 -nodes -sha256 -newkey rsa:2048 -subj "$SUBJECT_CA"-days 365 -keyout ca.key -out ca.crt
}

functiongenerate_server() {
   echo"$SUBJECT_SERVER"
   openssl req -nodes -sha256 -new -subj "$SUBJECT_SERVER"-keyout server.key -out server.csr
   openssl x509 -req -sha256 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 365
}

functiongenerate_client() {
   echo"$SUBJECT_CLIENT"
   openssl req -new -nodes -sha256 -subj "$SUBJECT_CLIENT"-out client.csr -keyout client.key 
   openssl x509 -req -sha256 -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 365
}

generate_CA
generate_server
generate_client
```


### Create mosquitto password file
`touch mosquitto/config/mosquitto.passwd`

### Start Docker-Compose
`docker-compose up`

### Change/Set Password for mosquitto
```
docker ps
docker exec -it [CONTAINER-ID]  sh

#s/> mosquitto_passwd -b [PASSWORDFILE] [USER] [PASSWORD]
#s/> mosquitto_passwd -b mosquitto.passwd SOMEUSER xxxxxxxx
#s/> exit

docker-compose up
```


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

#Postgres-Server
POSTGRES_USER=postgres
POSTGRES_PASSWORD=xxxxxxxxxx

```
