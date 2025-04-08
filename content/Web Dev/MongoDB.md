[[Web Dev/index|index]]

# In Node.js :

> .find() returns a cursor so use .find().forEach()

# MONGOOSE : 

```
         --> Collection C1 --> { Documnets ==> D1 , D2 , D3 }
DataBase --> Collection C2 --> { Documnets ==> D1 , D2 , D3 }
         --> Collection C3 --> { Documnets ==> D1 , D2 , D3 }

Every Collection on MongoDB ==> Every Model in Mongoose 
Schema in Mongoose define schema of documents on MongoDB
```

## Connecting to Mongo By Mongoose :

```node.js
const mongoose = require('mongoose');

const MONGO_URL = "mongodb+srv:// something ....";

await mongoose.connect(MONGO_URL,{})
.catch(err => console.log(err))
.then(() => console.log("connected to DB"));
console.log("after connection");

// Below is code add event listener :
mongoose.connection.on('error', err => {
	console.error(err);
});
```

## Schema and Model in Mongoose :
refer : [Mongoose v8.3.3: Schemas (mongoosejs.com)](https://mongoosejs.com/docs/guide.html)
```Javascript
const mongoose = require('mongoose');

const planetSchema = new mongoose.Schema({
   kepler_name : {
        type : String,
        required: true
   }
})

module.exports = mongoose.model('Planet', planetSchema);
// so here instead of Planet name as collection planets
// will be added to mongodb
```

## Manipulating Data in Docs :
>Note : UPSERT (Update if found, Insert new doc if not found)

> updateOne() function also add $setOnInsert property which can reveal that we are using MongoDB upsert, to avoid this use findOneAndUpdate() function. 
```Javascript
await planets.updateOne(
	{find_by_field : old_data} ,
	{field_to_be_changed : new_data} ,
	{upsert: true}
);

// UPSERT ==> Update + Insert
// Update if found, Insert new doc if not found
``` 

> Note : __v value is added by Mongoose 
```JSON
// (A document in MongoDB)
{
	"_id": ObjectId('6638dc913b33276166ad43b4'),
	"field":"data",
	"__v": 0
}
```
```
here __v is version key ==> version of document we created
if we change schema and want old and new data to persist then 
we can increase __v value differentiating new docs.
```
# MongoDB Queries :
## Manipulating Docs of Collection :
- ```db.collection_name.insertOne({ something in json format })``` to insert a single json object in form of document

>NOTE :: MongoDB automatically adds an ObjectId to each document

- ```db.collection_name.insertMany([{ something in json format } ,{ something in json format } ,{ something in json format } ,{ something in json format } .....])``` to insert a single json object in form of document
- ```db.collection_name.find()``` to grab all documents
- ```db.collection_name.find( {field1 : "value" , field2 : "value"} )``` to grab filtered documents according to given field/s and its value/s

> NOTE :  let `numbers1: [{1} ,{2} ,{3}]` and `numbers2: {1}` 
> then `.find({numbers: "1" })` will return numbers1 and numbers2 documents but if we do query such as `.find( {numbers : ["1"] } )` returns numbers2 document only

- ```db.collection_name.find( {field1 : "value" , field2 : "value"} , {field_required1 : 1 , field_required2 : 1} )``` to grab filtered documents according to given field/s and its value/s and to show only field_required field of each filtered document
- ```db.collection_name.find( {field1 : "value"} ).count()``` to grab filtered documents according to given field/s and its value/s and return only number of such documents
- ```db.collection_name.find( {field1 : "value"}).limit(n)``` to grab filtered documents according to given field/s and its value/s and to show only first "n" documents
- ```db.collection_name.find().sort( {field : 1} )``` to grab documents in ascending sorted form according to given field
- ```db.collection_name.find().sort( {field : -1} )``` to grab documents in descending sorted form according to given field


## Query Operators :
- to load query such as greater than $gt , greater than equal to $gte use dollar sign and always use a query inside curly brackets

>Greater or lesser operator
- ```db.collection_name.find( {rating: {$gt:7} } )``` query for greater than 7
- ```db.collection_name.find( {rating: {$lt:7} } )``` query for lesser than 7  
- ```db.collection_name.find( {rating: {$gte:7} } )``` query for greater than or equal to 7
-  ```db.collection_name.find( {rating: {$gte:7} } )``` query for lesser than or equal to 7

>OR operator
- ```db.collection_name.find( {$or: [{rating:7} , {rating:9}] )``` query for rating: 7 or rating: 9

>IN operator and NOT IN operator (for range of value)
- ```db.collection_name.find( {rating: {$in: [7,8,9] }} )``` query for rating: 7 or rating:8 or rating: 9
- ```db.collection_name.find( {rating: {$nin: [7,8,9] }} )``` query for documents which don't have rating: 7 or rating:8 or rating: 9

>ALL operator
- let `numbers1: [{1} ,{2} ,{3}]` and `numbers2: {1}` 
then `.find({numbers: {$all: ["1","2"]} })` will return numbers1 , it checks if the given field atleast contains the given values


## Nested Documents :
- let ```details:{ name:"XYZ" , email:"xyz@gmail.com" , age :"30"}```
then to find name XYZ use ```.find({ "details.name" : "XYZ" })``` 


## Deleting Docs of Collection :
- ```db.collection_name.deleteOne({ field: "value" })``` to delete a single json object in form of document
-  ```db.collection_name.deleteMany({ field: "value" })``` to delete multiple documents according to field


## Update Docs of Collection :
- ```db.collection_name.updateOne({ field_to_find: "value" } , {$set: {field_1_to_be_updated : "new_value_1" , field_2_to_be_updated : "new_value_2"}})``` to update a document which contain "field_to_find" field with value  = value and set "field_to_be_updated" field to its new value
- ```db.collection_name.updateMany({ field_to_find: "value" } , {$set: {field_1_to_be_updated : "new_value_1" , field_2_to_be_updated : "new_value_2"}})``` to update all document which contain "field_to_find" field with value  = value and set "field_to_be_updated" field to its new value

> Note : UPSERT (Update if found, Insert new doc if not found)
```node.js
await planets.updateOne(
	{kepler_name : data.kepler_name} ,
	{kepler_name : data.kepler_name} ,
	{upsert: true}
);

// UPSERT ==> Update + Insert
// Update if found, Insert new doc if not found
```
## Extra bits :
> INC (increment) operator 
> $inc: n to increment or $inc: -n to decrement
- ```db.collection_name.updateOne({ field_to_find: "value" } , {$inc: {field_to_be_updated : n })``` to increment a value by n in a document which contain "field_to_find" field with value  = value and set "field_to_be_updated" field to its new value

> PULL (to remove an element from array)
- ```db.collection_name.updateOne({ field_to_find: "value" } , {$pull: {array : "element_to_be_removed" })``` to remove element_to_be_removed from an array where there is a "field_to_find" field with value = value 

> PUSH (to add an element from array)
- ```db.collection_name.updateOne({ field_to_find: "value" } , {$push: {array : "element_to_be_added" })``` to add element_to_be_added to an array where there is a "field_to_find" field with value  = value 

> EACH (add or remove multiple elements of an array)
- ```db.collection_name.updateOne({ field_to_find: "value" } , {$push: {array : {$each: ["element1","element2","element3"]} })``` to add element1 2 3 to an array where there is a "field_to_find" field with value  = value 


# IN TERMINAL :
## Start terminal :
Type ```mongosh``` to enter mongodb shell

## Basic commands :
- ```cls``` to clear terminal
- ```show dbs``` to see all databases
- ```db``` to show all collections of database you are in
- ```help``` for help
- ```exit``` to exit
- ```use database_name``` to move to database_name database
- ```use database_name```  then ```db.collection_name``` to move to collection_name collection
