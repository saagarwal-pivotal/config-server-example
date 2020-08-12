# cloud-config-pivotal-cloud-foundry


Create a configuration file cfg.json

{  
  "git":{  
    "uri":"https://github.com/saagarwal-pivotal/config-server.git",
    "label":"master",
    "searchPaths":"master"
  }
}
Register the config-server

$ cf create-service -c cfg.json p-config-server trial my-config-server-satya


# Below are the commands to create credhub and bind it to app 

```
cf create-service credhub default my-credhub-instance -c secret.json

cf bind-service MY-APP  my-credhub-instance

cf restart MY-APP

cf env MY-APP
```

# Below are the commands to create cups and bind it to app

When you create a user provided service in the following way


```
cf cups ups-example1 -p '{"user":"user1", "password":"password1"}'
```

and bind that to your application, the information provided in the user provided service gets mapped into your VCAP_SERVICES environment variable.

It should look something like

```
{
  "user-provided": [
   {
    "credentials": {
     "password": "password1",
     "user": "user1"
    },
    "label": "user-provided",
    "name": "ups-example1"
   }
  ]
}
```

To map these properties into a Java object you can use @ConfigurationProperties
```
@Configuration
public class UserProvidedServicesProperties {

  @Autowired
  private UserProvidedServiceOneProperties userProvidedService1;

  // getters & setters

  @Configuration
  @ConfigurationProperties("vcap.services.ups-example1.credentials")
  public static class UserProvidedServiceOneProperties {
    private String user;
    private String password;

    // getters & setters
  }

```

# How to run the app

```
mvn install -DskipTests
cf push
```