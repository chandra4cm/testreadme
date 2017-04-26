# QueryBuilder

The QueryBuilder class is implemented around fluent interface pattern.

When calling `.and` , `.or`  the `QueryBuilder` logical mode is changed respectively.

Future calls to `where` adds the predicates to respective logical mode.

`QueryBuilder`'s default mode is `and`.


## Usage

#### mode changes on `QueryBuilder`
``` javascript

 querybuilder
 	.and() // sets the logical mode  to `and`
	 .where('a').eq(1) 	// alternate way : .where({a:1})
	 .where('b').eq(2) 	// alternate way : .where({b:2})
	 			// both 'wheres' can be combined as .where([{a:1} , {b:2}])
	 .or()// set the logical mode to `or`
	 .where('c').eq(3)
	 .where('d').eq(4);

 /*
     above results in the following equivalent `mongodb` query

     {
     	'$and': [{a:1} , {b:2}] ,
     	'$or':  [{c:3} , {d:4}]}

  */
```

#### deafult mode
``` javascript

 querybuilder
 	.where('a').eq(1)
 	.where('b').eq(2)
 	.where('c').eq(3);

/*
     above results in the following equivalent `mongodb` query

     {
     	'$and': [{a:1} , {b:2} , {c:3}]]
     }

  */
```

#### nested queries
``` javascript
 querybuilder
	.or()
	.where(sq =>{ // sq new `QueryBuilder`
		sq
		 .where('a').eq(1)
		 .where('b').eq(2);
	})
	.where(sq=>{ // sq new `QueryBuilder`
		sq
		 .where('c').eq(3)
		 .where('d').eq(4);
	});

/*
     above results in the following equivalent `mongodb` query

     {
     	'$or': [
     		{$and : [{a:1} , {b:2}]},
     		{$and : [{c:3} , {d:4}]}
     	]
     }

  */

```

#### skip / limit
``` javascript
querybuilder
	.where({a:1})
	.skip(1)
	.limt(2)

```
#### update query
``` javascript
// update all the documents where a = 1, sets the properties passed in update.
// default : updates the first matching document (mutli = false)
querybuilder
    .where({a:1})
    .update({b:1 , c:2})
    .update({d:4})
    .multi(true);

```


<!-- Start querybuilder.js -->

## QueryBuilder

QueryBuilder interface, to be implemened for  respective database systems.
 Implementation classes encapsulate the query building logic.

### Params:

* **Object** *[options]*
* **number** *[options.skip]*
* **number** *[options.limit]*

### Return:

* **QuerBuilder** instance

## collection(collectionName)

Sets the collection name

### Params:

* **string** *collectionName* collection name

### Return:

* **QueryBuilder** this this object for allowing method chaining

## where([path], [val])

Specifies a `path` for use with chaining.
Helps in building subqueries / clauses

#### Usage
```javascript
    query.where('age').gte(21).lte(65)

    // chaining
    query
    .where('age').gte(21).lte(65)
    .where('name', /^vonderful/i);

   //subquery
   query.where(subquerybuilder =>{
     subquerybuilder.or({a:1 , b:2})
   });
```

### Params:

* **String** *[path]*
* **Object** *[val]*

### Return:

* **QueryBuilder** this

## eq(val)

Specifies the complementary comparison value for paths specified with `where()`

#### Usage

    User.where('age').eq(49);

    // is the same as

    User.where('age', 49);

### Params:

* **Object** *val*

### Return:

* **QueryBuilder** this

## and()

#### Usage
sets the logical model to `and` and adds the clauses to `and`
```
    querybulider.and({a:1})

    querybuilder.and([{b:2},{c:3}])
```

### Params:

* **Object|Array.\<Object>|QueryBuilderFn** **

### Return:

* **QueryBuilder** this

## or()

#### Usage
sets the logical model to `or` and adds the clauses to `or`
```
    querybulider.or({a:1})

    querybuilder.or([{b:2},{c:3}])
```

### Params:

* **Object|Array.\<Object>|QueryBuilderFn** **

### Return:

* **QueryBuilder** this

## gt([path], val)

Specifies a $gt query condition.

When called with one argument, the most recent path passed to `where()` is used.

#### Usage
```
    querybuilder.where('age').gt(21)

    querybuilder.where('age').gt('age', 21)
```

### Params:

* **String** *[path]*
* **Number** *val*

## gte([path], val)

Specifies a $gte query condition.

When called with one argument, the most recent path passed to `where()` is used.
#### Usage
```
    querybuilder.where('age').gte(21)

    querybuilder.where('age').gte('age', 21)
```

### Params:

* **String** *[path]*
* **Number** *val*

## lt([path], val)

Specifies a $lt query condition.

When called with one argument, the most recent path passed to `where()` is used.

### Params:

* **String** *[path]*
* **Number** *val*

## lte([path], val)

Specifies a $lte query condition.

When called with one argument, the most recent path passed to `where()` is used.

### Params:

* **String** *[path]*
* **Number** *val*

## ne([path], val)

Specifies a $ne query condition.

When called with one argument, the most recent path passed to `where()` is used.

### Params:

* **String** *[path]*
* **Number** *val*

## in([path], val)

Specifies an $in query condition.

When called with one argument, the most recent path passed to `where()` is used.

### Params:

* **String** *[path]*
* **Number** *val*

## nin([path], val)

Specifies an $nin query condition.

When called with one argument, the most recent path passed to `where()` is used.

### Params:

* **String** *[path]*
* **Number** *val*

## size([path], val)

Specifies a $size query condition.

When called with one argument, the most recent path passed to `where()` is used.

### Params:

* **String** *[path]*
* **Number** *val*

## limit(val)

Specifies the limit option.

#### Usage
```javascript
    query.limit(20)
```

### Params:

* **Number** *val*

## skip(val)

Specifies the skip option.

#### Usage
```javascript
    query.skip(100).limit(20)
```

### Params:

* **Number** *val*

## upsert(val)

Specifies the upsert option.
To create the document as part of update operation if the document does not exist.

#### Usage
```javascript
    query.upsert(true)
```
#### Note
        Only use it during updates

### Params:

* **Boolean** *val*

## multi(val)

Specifies the multi option.
To updates all the documents that match the query selector.

#### Usage
```javascript
    query.multi(true)
```
#### Note
        Only use it during updates

### Params:

* **Boolean** *val*

## serialize()

serializes the querybuilder

### Return:

* **Object** queryjson

* **Object** queryjson.query query

* **Object** queryjson.options query/update options

* **Object** queryjson.doc holds the properties to update, captures via {QueryBuilder.update}

## printQuery()

prints the query in entirety, view it to see what is being published

## update(props)

collects the properties to be updated/set
#### Usage
```javascript

querybuilder.
    .where({a:1})
    .update({b:1 , c:2});
//set the properties
```

### Params:

* **Object** *props* properties to set

### Return:

* **QueryBuilder** this

## QueryBuilderFn(subquery)

SubQuery function to build complex queries

### Params:

* **QueryBuilder** *subquery*

<!-- End querybuilder.js -->
