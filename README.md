# ArchAngel
Simple Microservice Project: ArchAngel simulates a front-end composed of a single jsp and a "widget" that enabled http injection, a microservice running on a sererate IP and container (instance) which takls to a pre-seeded MongoDB container

### Short and sweet

build the project with `./bootstrap`

then start it with `docker-compose up -d`

You can how point your browser to the IP the services were started on and it will come up. 
For example: 

```
http://localhost/
```

## [NOTE] I spent a long time working on this project and it went through a number of 'phases' ... the stuff below may or may not be applicable but I'll leave it in the as a reminder to where it began and where it is today


### Open Firewall Ports
Open the selected port, but first find which rule is enabled
```
firewall-cmd --get-active-zones
```

Then remember to reload the firewall for changes to take effect.
```
firewall-cmd --zone=public --add-port=2888/tcp --permanent
firewall-cmd --reload
```



For gitbucket PR's and commits:
http://sprintkins:8080/jenkins/job/archangelui-system/build?token=<secret token>


log out, log back in ( or refresh your profile . ~/.profile ))  
Once loaded into your shell, you can call it with:  
```
$ docker-compose-restart [SERVICE...]
```

... OR ... you can use this command line option (which does the same thing)  
```
docker-compose up -d --no-deps --build <service_name>
```

### ToDo

* containerize the Front-end and microservice (MongoDB completed)
* move the startup and overall control to docker-compose


### Important Note: If maven fails, exeute  
mvn versions:display-dependency-updates

### Index
* [Bootstrap](#bootstrap)

Clean the Environment
* [Build Application](#manually-build-and-start-the-application)
* [Clean the Environment](#clean-the-environment)
* [Build Web UI and Microservice](#build-web-ui-and-microservice)
* [Build Docker image](#build-docker-image)
* [Start the WebApp](#start-the-webapp)
* [Start the Microservice](#start-the-microservice)
* [Start MongoDB](#start-mongodb)
* [Testing](#testing)

#### Bootstrap
This section allows for the automated building and starting of the application without any general knowledge of docker, java or microservices

**To build the stable version (main branch)**

```
git clone https://github.com/ssgeejr/ArchAngel.git  
cd ArchAngel  
sh bootstrap
```

**To build the develop branch**

```
git clone https://github.com/ssgeejr/ArchAngel.git  
cd ArchAngel  
git pull origin develop  
sh bootstrap
```

### Manually build and start the application

**this assumes you have done the following:**  
*indifferent to the branch you are working with, ensure you are in the ArchAngel directory*

```
git clone https://github.com/ssgeejr/ArchAngel.git  
cd ArchAngel  
```

**To build the develop branch**

```
git clone https://github.com/ssgeejr/ArchAngel.git  
cd ArchAngel  
git pull origin develop  
```


### Clean the Environment

```
docker stop seededdb
docker rmi -f $(docker images | grep 'mongo:seeded' | awk '{print $3}')
kill -9 $(ps -ef | grep '[a]rchAngelService.jar' | awk '{print $2}')
kill -9 $(ps -ef | grep '[a]rchAngelFrontEnd.jar' | awk '{print $2}')
rm -rf .extract
rm -rf frontend/.extract
rm -rf service/.extract
```

#### Build Web UI and Microservice

```
mvn clean package  
```

#### Build Docker image

```
cd database/docker
docker build --tag mongodb:seeded .
```

#### Start the WebApp

```
cd frontend
java -jar target/archAngelFrontEnd.jar &
```

#### Start the Microservice

```
cd ../service
java -jar target/archAngelService.jar --httpPort=9000 & 
```

#### Start MongoDB

```
docker run --name seededdb --rm -ti -p 27017:27017 mongodb:seeded
```

#### Testing
Point your browser to http://localhost:8080  

You should see the Web UI, sleect a Car Model and Search for the data (only a single row will return)  
(if the instance is running on a remote server, substitute that IP for localhost)  


Push test 03302020
