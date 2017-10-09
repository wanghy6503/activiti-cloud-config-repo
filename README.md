# activiti-cloud-config-repo

Config repository to provide config files for some Activiti cloud projects. A spring cloud config server can be pointed at this repo to host the config files contained here.

Can be hosted using a standard spring cloud config server e.g. docker image https://hub.docker.com/r/hyness/spring-cloud-config-server/ and using environment variable SPRING_CLOUD_CONFIG_SERVER_GIT_URI=https://github.com/ryandawsonuk/activiti-cloud-config-repo

Each client will need to have a bootstrap.properties and the spring-cloud-starter-config dependency. The bootstrap.properties needs to provide at least spring.application.name (which should match to the property file in the config server) and the spring.cloud.config.uri to find the config server. The client will look for the config server very early in its startup.

Let's say we run the config server using name activiti-cloud-config-server on port 8888 (similar to https://github.com/hyness/spring-cloud-config-server/blob/master/docker-compose.yml). Then we could include this value in all of the bootstrap.properties:

spring.cloud.config.uri=http://activiti-cloud-config-server:8888

And in each of the clients its own application name will also feature in its bootstrap.properties:

spring.application.name=${ACT_RB_APP_NAME:runtime-bundle1}
spring.application.name=${ACT_GATEWAY_APP_NAME:gateway}
spring.application.name=${ACT_REGISTRY_APP_NAME:registry} 
spring.application.name=${ACT_AUDIT_APP_NAME:audit}
spring.application.name=${ACT_QUERY_APP_NAME:query}

Note that if a new runtime bundle (or any other app) is deployed with a new name (e.g. runtime-bundle2) then it will only find a config file in the repo if it is named runtime-bundle2.properties (or otherwise matches the convention based on app name and profile name).