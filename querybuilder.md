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

### Params:

* **String** *[path]* 
* **Object** *[val]* 

### Return:

* **QueryBuilder** this

## gt([path], val)

Specifies a $gt query condition.

When called with one argument, the most recent path passed to `where()` is used.

####Example

    Thing.find().where('age').gt(21)

    // or
    Thing.find().gt('age', 21)

### Params:

* **String** *[path]* 
* **Number** *val* 

## gte([path], val)

Specifies a $gte query condition.

When called with one argument, the most recent path passed to `where()` is used.

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

####Example

    query.limit(20)

### Params:

* **Number** *val* 

## skip(val)

Specifies the skip option.

####Example

    query.skip(100).limit(20)

### Params:

* **Number** *val* 

## upset(val)

Specifies the upsert option.
To create the document as part of update operation if the document does not exist.

####Example

    query.upsert(20)
####Note
		Only use it during updates

### Params:

* **Number** *val* 

## multi(val)

Specifies the multi option.
To updates all the documents that match the query selector.

####Example

    query.multi(true)
####Note
		Only use it during updates

### Params:

* **Boolean** *val* 
