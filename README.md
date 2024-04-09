# config-server
This is a config server. It is a spring boot application that provides configuration for other microservices services.
Explore more about spring config server on the [Wiki](https://docs.spring.io/spring-cloud-config/docs/current/reference/html/) and [Wiki](https://cloud.spring.io/spring-cloud-config/multi/multi__spring_cloud_config_server.html) for more information.

The config server can pull the data from different source like:
- Git
- File System
- Vault
- JDBC

Let's assume out **Git**  repository contain configuration file like this:

```shell
.
├── application.yml
├── nginx.conf
├── pop-ms.yml
├── pop-spa.yml
└── spa.json
```

Start the application
> Before start , Make sure , you have the correct github URL in the `application.yml` file.

```shell
mvn spring-boot:run
```
Supported endpoint to explore the config-server are as follows:

Format
```
/{application}/{profile}[/{label}]
/{application}-{profile}.yml #http://localhost:8888/config-server/development/main/application.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties 

```

Example
```http
# Will bring the properties from application.yml from respective branch
curl host:PORT/context/main
curl host:PORT/context/custom

# property available in application.yml with profiile development
curl host:PORT/context/development

# property available in application.yml with profiile development and git branch master
curl host:PORT/context/development/master

curl host:PORT/context/development,db/master

# http://localhost:8888/config-server/default/main/spa.json 
# http://localhost:8888/config-server/development/main/spa.json

curl host:PORT/context-development.yml
curl host:PORT/context-db.properties
curl host:PORT/master/context-db.properties
```


Endpoint available in this application are as follows:

```shell
http://localhost:8888/config-client/default
http://localhost:8888/config-client/default/main
http://localhost:8888/config-client/default/custom
http://localhost:8888/config-client/default/custom/application.yml
http://localhost:8888/config-client/default/custom/nginx.conf
http://localhost:8888/config-client/development/custom/nginx.conf
http://localhost:8888/config-client/default/custom/spa.json
http://localhost:8888/config-client/default/main/spa.json #default profile, main branch, spa.json file
http://localhost:8888/config-client/development/main/spa.json #development profile, main branch, spa.json file
http://localhost:8888/config-client/development/custom/spa.json #development profile, custom branch, spa.json file
```

Consume the JSON text with Javascript

```javascript
fetch('http://localhost:8888/config-server/default/main/spa.json')
  .then(response => response.text())
  .then(data => {
    console.log(data);
    console.log(JSON.parse(data));
  });
```