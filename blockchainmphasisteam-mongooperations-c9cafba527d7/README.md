# mongooperations

Mongodb Operations

## Run test script

### Clone the repository

```
git clone https://mphasisblockchain@bitbucket.org/blockchainmphasisteam/mongooperations.git

```

### Install dependencies
```
$ npm install

```

### Run test.js
```
$ node test.js

```

## How to use mongooperations module?


### Install mongooperations module

```
npm install git+https://mphasisblockchain:mY7pcGwpxkLSFXw3y7rf@bitbucket.org/blockchainmphasisteam/mongooperations.git

```

### Install mongooperations module as dependency

> To save module as a dependency use --save option

```
npm install git+https://mphasisblockchain:mY7pcGwpxkLSFXw3y7rf@bitbucket.org/blockchainmphasisteam/mongooperations.git --save

```



##### import mongodb module and mongooperations module 

```javascript
const mongoclient = require('mongodb').MongoClient;
const dboper = require('mongooperations');
```

##### Initialize mongodb url and databasename

```javascript
const url = "mongodb://localhost:27017";
const dbName = 'testdatabase';

```


##### Call mongooperations API

> NOTE: mongooperations API returns promise

```javascript
mongoclient.connect(url).then((client) => {
    //assert.equal(err, null);

    console.log("Connected correctly to the server");
    const db = client.db(dbName);

    dboper.insertDocument(db, {
            name: "vadonut",
            description: "Test"
        }, 'dishes')
        .then((result) => {

            console.log('Insert Document:\n', result.ops);
            console.log("Find all documents in the collection : ");
            return dboper.findDocuments(db, "dishes")

        })
        .then((docs) => {
            console.log("Found documents:\n ", docs);

            console.log("Update document : ");

            return dboper.updateDocument(db, {
                name: 'vadonut'
            }, {
                description: 'Updated Test'
            }, 'dishes')
        })
        .then((result) => {
            console.log("Updated Document:\n ", result.result);
        })
        .then(() => {
            let query = {name : 'vadonut' }
            console.log("Find document by query : \n");
            console.log("Searching by query : ",query);
            return dboper.findDocumentsByQuery(db, "dishes", query);
        })
        .then((docs) => {
            console.log("Found document :\n ", docs);

            console.log("Inserting multiple documents");
            var testList = [];
            var obj1 = {
                name: "vadonut2",
                description: "Test2"
            }

            var obj2 = {
                name: "vadonut3",
                description: "Test3"
            }
            testList.push(obj1);
            testList.push(obj2);

            //inserting testList
            return dboper.insertManyDocuments(db, testList, "dishes");
        })
        .then((result) => {
            
            console.log("Inserted : "+result);
            //find records after inserting many records
            return dboper.findDocuments(db, "dishes")

        })
        .then((docs) => {
            console.log("Docs : ",docs);
            console.log("Drop collection : \n");
            //return db.dropCollection('dishes')
            return dboper.dropCollection(db, 'dishes');
        })
        .then((result) => {
            console.log("Dropped collection: " + result);
            client.close();
        }).catch((err) => console.log("Error : " + err));

})
.catch((err) => console.log("Error : " + err));

```




