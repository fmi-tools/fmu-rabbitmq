# Rabbitmq FMU

This project builds a FMU which uses a rabbitmq server feed live or log data into a simulation.

The FMU is configured using a script TBD for the output variables that are model specific.

It can be configured by setting the following parameters:

```xml
<ScalarVariable name="config.hostname" valueReference="0" variability="fixed" causality="parameter">
    <String start="localhost"/>
</ScalarVariable>
<ScalarVariable name="config.port" valueReference="1" variability="fixed" causality="parameter">
    <Integer start="5672"/>
</ScalarVariable>
<ScalarVariable name="config.username" valueReference="2" variability="fixed" causality="parameter">
    <String start="guest"/>
</ScalarVariable>
<ScalarVariable name="config.password" valueReference="3" variability="fixed" causality="parameter">
    <String start="guest"/>
</ScalarVariable>
<ScalarVariable name="config.routingkey" valueReference="4" variability="fixed" causality="parameter">
    <String start="linefollower"/>
</ScalarVariable>
<ScalarVariable name="config.starttimestamp" valueReference="5" variability="fixed" causality="parameter">
    <String start="2019-01-04T16:41:25+0200"/>
</ScalarVariable>
<ScalarVariable name="config.communicationtimeout" valueReference="6" variability="fixed" causality="parameter" description="Network read time out in seconds">
    <Integer start="60"/>
</ScalarVariable>
```

## Dockerized RabbitMq
To launch a Rabbitmq server the following can be used:

```bash
cd server
docker-compose up -d
```
This will launch it at localhost `5672` for TCP communication and http://localhost:15672 will serve the management interface. The default login is username: `guest` and password: `guest`


# Building the project
The project uses CMake and is intended to be build for multiple platforms; Mac, Linux and Windows.

# Environment

A number of tools are required.

## Docker and dockcross

Make sure that docker is installed and that the current user has sufficient permissions.

Prepare dockcross helper scripts
```bash
# darwin
docker run --rm dockcross/darwin-x64:latest > ./darwin-x64-dockcross
chmod +x ./darwin-x64-dockcross

# linux
docker run --rm dockcross/linux-x64:latest > ./linux-x64-dockcross
chmod +x ./linux-x64-dockcross

# windows
docker run --rm dockcross/windows-static-x64:latest > ./win-x64-dockcross
chmod +x ./win-x64-dockcross
```

## Preparing dependencies
To compile the dependencies first make sure that the checkout contains submodules:

```bash
git submodule update --init
```

To compile the dependencies first prepare the docker 

```bash
mkdiir -p build

# run cmake
./<platform>-dockcross cmake -Bbuild/<platform> -H.

# compile
./<platform>-dockcross make -Cbuild/<platform> -j8
```