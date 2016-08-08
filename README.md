# *docker-rpi-mqtt*

## *This container can be used to start Mosquitto MQTT Broker*

## *Setup Config*

```
#!shell
    #!/bin/bash
    
    local scriptDir=${YOUR_DATA_DIR}/sources/scripts
    local buildDir=${THIS_REPOSITORY_DIR}
    
    local DOCKER_DATA_DIR=${YOUR_DATA_DIR}/dockerdatadir
    local MQTT_DATA_DIR=${DOCKER_DATA_DIR}/mqtt
    
    if [ ! -d "${DOCKER_DATA_DIR}" ]; then
        echo 'Creating ${DOCKER_DATA_DIR} to store MQTT Data...'
        mkdir -m 777 ${DOCKER_DATA_DIR}
    fi

    if [ ! -d "${MQTT_DATA_DIR}" ]; then
        echo 'Creating ${MQTT_DATA_DIR} to store MQTT Data...'
        mkdir -m 777 ${MQTT_DATA_DIR} ${MQTT_DATA_DIR}/config ${MQTT_DATA_DIR}/log ${MQTT_DATA_DIR}/data
    fi

    if [ ! -d "${MQTT_DATA_DIR}/config/mosquitto.conf" ]; then
        cp -rf ${buildDir}/config/* ${MQTT_DATA_DIR}/config
        chmod 777 ${MQTT_DATA_DIR}/config
        chmod 777 ${MQTT_DATA_DIR}/config/*
    fi


    echo "Running docker images toke/mosquitto"
    docker run -d -p 1883:1883 -p 9001:9001 \
            -v ${YOUR_DATA_DIR}/dockerdatadir/mqtt/config:/mqtt/config:ro \
            -v ${YOUR_DATA_DIR}/dockerdatadir/mqtt/log:/mqtt/log \
            -v ${YOUR_DATA_DIR}/dockerdatadir/mqtt/data/:/mqtt/data/ \
            --name mqtt toke/mosquitto

```