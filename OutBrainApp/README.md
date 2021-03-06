##  instructions

## install Docker on ubuntu 20.04
apt-get install -y docker docker-compose

## download ubuntu:latest image:
docker pull ubuntu:latest

## Build myapp:latest according to Dockerfile inside OutbrainApp Directory:
cd OutBrainApp
docker build -t myapp:latest .

## Run Docker cntainer:
docker run -d -p 88:88 myapp:latest

## Determine IP Address (Run in bash under Docker Host):
CID=$(docker ps -a | grep myapp:latest | grep -iv exited | awk '{print $1}')

MYAPP_ADDRESS=$(docker inspect ${CID} | grep IPAddress | cut -d '"' -f 4 | tail -1)

## For driveStatus POST json file:
curl -X POST http://${MYAPP_ADDRESS}:88/v1/api/driveStatus -H 'Content-Type: application/json' -d @input.json

## For driveStatus GET Complete result:
curl -X GET http://${MYAPP_ADDRESS}:88/v1/api/driveStatus -H 'Content-Type: application/json' 
 
## For driveStatus GET Online result(same for Offline):
curl -X GET http://${MYAPP_ADDRESS}:88/v1/api/driveStatus?drive_status=Online -H 'Content-Type: application/json'

## For checkCurrentWeather GET Current city by IP:
curl -X GET http://${MYAPP_ADDRESS}:88/v1/api/checkCurrentWeather -H 'Content-Type: application/json'

## For checkCurrentWeather GET Cuurent Weather by City:
curl -X GET -G http://${MYAPP_ADDRESS}:88/v1/api/checkCityWeather -H 'Content-Type: application/json' --data-urlencode "city=<city_name>"

