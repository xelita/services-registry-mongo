# services-registry-mongo
MongoDB queries for Services-Registry application.

## Launch the mongoDB instance
``` bash
mongod --dbpath /Users/Benjamin/Documents/dev/projects/sources/xelita/github/services-registry-mongo/data/
```

## Import data in the application collection
``` bash
mongoimport --db registry -c application --file registry.json 
```

## Launch the mongo shell
``` bash
mongo
use registry
```

## MongoDB queries

### getApplications

Find all registered applications with their configurations.


``` bash
db.application.find();
```

### getApplication

Find a registered application with its configurations.


``` bash
db.application.findOne({app: "app"});
```

### addApplication

Create a new application with or without configurations.

``` bash
db.application.insert({app: "app", configs: "[]"})
```

### updateApplication:

update configurations of an existing application.

``` bash
db.application.update({app: "app"}, {$set: {configs: [{env: "dev", data: [{apiUrl: "http://localhost:1337/api/v3"}]}]}})
```

### removeApplication:

Remove an existing application.

``` bash
db.application.remove({app: "app"}, {justOne: true})
```
### setConfigs:

set configurations to an existing application.

``` bash
db.application.update({app: "app"}, {$set: {configs: [{env: "dev", data: [{apiUrl: "http://localhost:1337/api/v3"}]}]}})
```
















