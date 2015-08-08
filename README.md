# services-registry-mongo
MongoDB queries for Services-Registry application.

## Launch the mongoDB instance
``` bash
mongod --dbpath /Users/Benjamin/Documents/dev/projects/sources/xelita/github/services-registry-mongo/data/
```

## Import data in the application collection
``` bash
mongoimport --db registry -c application --file registry.json --jsonArray 
```

The structure of a document is the following:
``` bash
[
   {
      "app": "stw",
      "env": "dev",
      "key": "apiUrl",
      "value": "http://localhost:1337"
   },
   {
      "app": "stw",
      "env": "dev",
      "key": "appToken",
      "value": "ssk%2')*e"
   },
   {
      "app": "stw",
      "env": "staging",
      "key": "apiUrl",
      "value": "http://localhost:1337"
   },
]
```

## Launch the mongo shell
``` bash
mongo registry
```

## MongoDB queries on application configurations

### getAppConfigs

Find all configurations of a specific application.


``` bash
db.application.aggregate([
   {$match: {app: app}},
   {$group: {_id: '$env', configs: {$push: {key: '$key', value: '$value'}}}},
   {$project: {_id: 0, env: '$_id', configs: 1}}
])
```

### setAppConfigs

Set application configurations (removing the existing ones).

``` bash
```

### removeAppConfigs:

Remove all configurations that belong to an application.

``` bash
db.application.remove({app: "app"})
```

## MongoDB queries on applications configurations targetting a specific environment

### getEnvConfigs:

Get the configurations registered to a specific environment.

``` bash
db.application.find({app: app, env: env}, {_id: 0, app: 0, env: 0})
```

### setEnvConfigs

Set configurations to a specific environment.

``` bash
```

### removeEnvConfigs

Remove the configurations registered to a specific environment.

``` bash
db.application.remove({app: app, env: env})
```

## MongoDB queries on environment configuration values

### getConfig:

Get the configuration value set for a configuration key.

``` bash
db.application.findOne({app: app, env: env, key: key}, {_id: 0, app: 0, env: 0, key: 0});
```

### setConfig:

Set a configuration key to an application environment.

``` bash
db.application.update({app: app, env: env, key: key}, {app: app, env: env, key: key, value: value}, {upsert: true})```

### removeConfig:

Remove a configuration key from the configurations list.

``` bash
db.application.remove({app: app, env: env, key: key});
```





