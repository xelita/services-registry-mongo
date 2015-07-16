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
db.application.find({}, {_id: 0})
```

### getApplication

Find a registered application with its configurations.


``` bash
db.application.findOne({app: 'app'}, {_id: 0})
```

### addApplication

Create a new application with or without its configurations.
Update (with 'upsert' set to true) is used instead of usual insert because it avoids to create an application several times.

``` bash
db.application.update({app: 'app'}, {app: 'app', configs: '[]''}, {upsert: true})
```

### updateApplication:

Update the configurations of an existing application.

``` bash
db.application.update({app: "app"}, {$set: {configs: [{env: "dev", data: [{apiUrl: "http://localhost:1337/api/v3"}]}]}})
```

### removeApplication:

Remove an existing application.

``` bash
db.application.remove({app: "app"})
```

## MongoDB queries on applications configurations

### getConfigs:

Get the configurations of a specific application.

``` bash
db.application.findOne({app: "app"}, {_id: 0, configs: 1})
```

### setConfigs:

Set configurations to an existing application.

``` bash
db.application.update({app: "app"}, {$set: {configs: [{env: "dev", data: [{apiUrl: "http://localhost:1337/api/v3"}]}]}})
```

### removeConfigs:

Remove configurations from a specific application.

``` bash
db.application.update({app: "app"}, {$set: {configs: []}})
```

## MongoDB queries on applications configurations targetting a specific environment

### getEnvConfigs:

Get configurations from a specific environment.

``` bash
db.application.aggregate([{$unwind: '$configs'}, {$match: {app: 'app', 'configs.env': 'dev'}}, {$project: {_id: 0, env: '$configs.env', data: '$configs.data'}}])
```

### setEnvConfigs

Set configurations to a specific environment.

``` bash
db.application.update({app: 'app'}, {$pull: {configs: {env: 'dev'}}})
db.application.update({app: 'app'}, {$push: {configs: {env: 'dev', data: []}}})
```

### removeEnvConfigs

Remove the registered configurations from a specific environment.

``` bash
db.application.update({app: 'app', 'configs.env': 'dev'}, {$pull: {configs: {env: 'dev'}}})
```

## MongoDB queries on environment configuration values

### getEnvConfig:

Get the configuration value from a configuration key.

``` bash
db.application.aggregate([{$unwind: '$configs'}, {$unwind: '$configs.data'}, {$match: {app: 'stw', 'configs.env': 'dev', 'configs.data.apiUrl': {$exists: true}}}, {$project: {_id: 0, 'apiUrl': '$configs.data.apiUrl'}}])
```








