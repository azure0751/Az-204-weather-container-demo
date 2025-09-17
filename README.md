# Az-204-weather-container-demo





## Run the Front End Application Container

```bash
docker run -d -p 8090:8080 \
  -e ASPNETCORE_ENVIRONMENT=Development \
  -e DOTNET_USE_POLLING_FILE_WATCHER=1 \
  -e DOTNET_RUNNING_IN_CONTAINER=true \
  -e DOTNET_HOST_PATH=/usr/share/dotnet/dotnet \
  -e "WeatherUrl=http://weatherapi-container:8080/weatherforecast" \
  --name weatherfronapp1-container \
  weatherfront:1.0
```


## Docker Compose file , if you want to Build the docker image and Run the containr

Run the Command  : 

```bash
docker-compose up --Build
```
Below is Docker  Compose yaml File

```bash
version: '3.9'

services:
  weatherapi:
    build:
      context: ./wetherforecastapi   # path where Dockerfile for weatherapi exists
      dockerfile: Dockerfile
    image: weatherapi:1.0
    container_name: weatherapi-container
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_URLS=http://+:8080
    networks:
      - weather-net

  weatherfront:
    build:
      context: ./WeatherFront   # path where Dockerfile for weatherfront exists
      dockerfile: Dockerfile
    image: weatherfront:1.0
    container_name: weatherfront-container
    ports:
      - "8090:8080"
    environment:
      - DOTNET_USE_POLLING_FILE_WATCHER=1
      - DOTNET_RUNNING_IN_CONTAINER=true
      - DOTNET_HOST_PATH=/usr/share/dotnet/dotnet
      - WeatherUrl=http://weatherapi-container:8080/weatherforecast
    depends_on:
      - weatherapi
    networks:
      - weather-net

networks:
  weather-net:
    driver: bridge
```


## If you want to Run the container without Building the images 

You can also specify the file name of yaml.

```bash
docker-compose -f docker-compose-withoutBuild.yml up -d
```
Content of the file 

```bash
version: '3.9'

services:
  weatherapi:
    image: weatherapi:1.0
    container_name: weatherapi-container
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_URLS=http://+:8080
    networks:
      - weather-net

  weatherfront:
    image: weatherfront:1.0
    container_name: weatherfront-container
    ports:
      - "8090:8080"
    environment:
      - DOTNET_USE_POLLING_FILE_WATCHER=1
      - DOTNET_RUNNING_IN_CONTAINER=true
      - DOTNET_HOST_PATH=/usr/share/dotnet/dotnet
      - WeatherUrl=http://weatherapi-container:8080/weatherforecast
    depends_on:
      - weatherapi
    networks:
      - weather-net

networks:
  weather-net:
    driver: bridge
```
