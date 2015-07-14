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

## MongoDB queries on application

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

## MongoDB queries on applications configurations

### getConfigs:

Get the configurations of a specific application.

``` bash
db.application.findOne({app: "app"}, {_id: 0, configs: 1})
```

### setConfigs:

Set the configurations of a specific application.

``` bash
db.application.update({app: "app"}, {$set: {configs: [{env: "dev", data: [{apiUrl: "http://localhost:1337/api/v3"}]}]}})
```

### removeConfigs:

Remove the configurations from a specific application.

``` bash
db.application.update({app: "stw"}, {$set: {configs: []}})
```

## MongoDB queries on applications configurations targetting a specific environment

### getEnvConfigs:

Get the configurations of an application for a specific environment.

``` bash
db.application.aggregate([{$unwind: '$configs'}, {$match: {'configs.env': 'dev'}}, {$project: {_id: 0, env: '$configs.env', data: '$configs.data'}}])
```

### setEnvConfigs

Set the configurations for a specific environment.

``` bash
db.application.update({app: 'stw', 'configs.env': 'dev'}, {$set: {'configs.$': {env: 'dev', data: []}}}, { multi: true})
```

### removeEnvConfigs

Remove the registered configurations from a specific environment.

``` bash
db.application.update({app: 'stw', 'configs.env': 'dev'}, {$pull: {configs: {env: 'dev'}}})
```











