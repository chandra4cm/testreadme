# CM DAL ( Database Abstraction Layer)

**CM DAL** is a `node.js` database abstraction that hides the specifics of the database technology from the applications.

Its purpose is to make the applications DB agnostic. Switching database technologies must involve no changes to the applications.

Currently abstracts `mongodb`.

## Installation

```bash

> npm install cmdal
```

## Usage

```javascript
var CivilMapsDAL = require('cmdal').CivilMapsDAL;
var ds           = CivilMapsDAL.getDatasource({mydb : { type:'mongo' ,'url' : 'mongodb://localhost:27017/testdb01'}}, 'mydb'); //jshint ignore:line

ds.on('init' , function(){
   ds.insert({a:1 , b:2 , c:3}, {collection : 'testcollection'})
   .then(function(){
       var query = ds.Query();

       query.collection('testcollection')
               .where('a').eq(1);
               // .multi(true);

       return ds.find(query);

   })
   .then(console.log)
   .then(function(){});

});

```

## CivilMapsDAL

DAL factory

#### getDatasource(config, dsName)

getDataSource , a factory method to return a driver class that implements {CivilMapsDatabase}

###### Example :
```
var config =  {'mydb' :{url:"mongodb://localhost:27017/testdb"}};
var dsName = 'mydb'; // identifies the configuration secion in config;

var CivilMapsDAL = require('cmdal').CivilMapsDAL;
var ds           = CivilMapsDAL.getDatasource({mydb : { type:'mongo' ,'url' : 'mongodb://localhost:27017/testdb01'}}, 'mydb'); //jshint ignore:line

```

###### Params:

* **Object** *config* holds the configuration for the datasources
* **string** *dsName* the name of the datasource as known to the application

###### Returns:

* **CivilMapsDatabase** - specific driver implementation
-----

## CivilMapsDatabase

CivilMapsDatabase, interface. This is returned on **CivilMapsDAL.getDatasource**.

Emits event *init* on succesful initialization. Calling any method prior to this event will result errors.

Emits event *error* on failure to initialize. Add a listener to this event to handle errors.

#### Query()
--
Factory method , returns the driver specific **QueryBuilder**.

**QueryBuilder** , provides a fluent API for query building. Refer to the QueryBuilder API for query building.
[QueryBuilder fluent api](querybuilder.md)

###### Return:

* **QueryBuilder** - a fluent query builder  to construct queries

#### insert(doc, options, options.collName)

Inserts a document into request collection

###### Params:

* **Object** *doc* document to be inserted
* **Object** *options* insert options
* **string** *options.collName* collection to insert into

###### Return:

* **Promise**

#### find(query)

finds documents

###### Params:

* **QueryBuilder** *query* encapsulates the query

###### Return:

* **Promise**

#### update(query)

updates document(s) as

###### Params:

* **QueryBuilder** *query* encapsulates the query

###### Return:

* **Promise**

* **Promise.reject** - if query is empty

#### remove(query)

remove document(s)

###### Params:

* **QueryBuilder** *query* encapsulates the query

###### Return:

* **Promise**

- **Promise.reject** - if query is empty
