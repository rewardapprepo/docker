The project has the following services
- Config Server
- Discovery
- Gateway
- Reward Service
- User Service


Database:
Used PostgreSQL as the database of the services. There is a file for initializing databases called init.sql. When starting docker-compose, this file will be copied to the container for initializing userdb and rewarddb.

Config Server:
Spring Cloud Config server is created for storing properties

Discovery:
Netflix Eureka is used as a load balancer

Gateway:
Netflix Zuul is used for gateway

Services are communicating with OpenFeign each other

Images are pushed to the docker hub. You can access with following image-name:tag
- dogancancelik/discovery:latest
- dogancancelik/gateway:latest
- dogancancelik/config-server:latest
- dogancancelik/user-service:0.0.1
- dogancancelik/reward-service:0.0.1


NOTE: 
- There is an error with the docker-compose file. I need time for solving the database connection issue from other containers like user-service and reward-service for the docker container. Services are working locally.
- I configured Spring Cloud Config Server for giving database properties from outside of the services but It has also an error. 
- For running the project, first you need to start PostgreSQL with the below docker-compose.yml. (init.sql file should be in the same directory with the docker-compose.yml file)
- After cloning all projects you need to start services in the following order DiscoveryService, GatewayService, UserService, and RewardService.
- There are config server configurations in the projects but I couldn't solve fetching the database configurations problem so I didn't use it for general. 
- Mostly I focused on the project architecture during the project so I couldn't write unit tests.
- My aim was starting the project with one docker-compose and fetching properties from https://github.com/rewardapprepo/RewardAppConfiguration repository 

rewardapp_postgresql:
    image: postgres
    container_name: rewardapp_postgresql
    volumes:
        - ./init.sql /docker-entrypoint-initdb.d/init.sql
    environment:
        - POSTGRES_MULTIPLE_DATABASES=userdb,rewarddb
        - POSTGRES_USER=coffeeapp
        - POSTGRES_PASSWORD=coffeeapp
        - POSTGRES_INITDB_ARGS= "-A md5"
    ports:
      - "5432:5432"


