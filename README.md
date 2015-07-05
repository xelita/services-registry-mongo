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

### addApplication
``` bash
db.application.insert({app: "app", configs: "[]"})
```

### updateApplication:
``` bash
db.application.update({app: "app"}, {$set: {configs: [{env: "dev", data: [{apiUrl: "http://localhost:1337/api/v3"}]}]}})
```

### removeApplication:
``` bash
db.application.remove({app: "app"}, {justOne: true})
```