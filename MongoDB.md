## MongoDB
Document Database
Stores JSON data
Schema is Optional
BSON format : 

Why MongoDB
Relation data base - Storing optional fields. Sparse data.

Single document stores all data for the record. **Data accessed together , stays together.** So no need of relations between records.


MongoDB - Community Edition

mongosh


databases(appdb)
collections( similar to tables) (users)
Documents(row)
Field( Column)

Every Document can have its own Schema.

show dbs
use appdb
db.users.insertOne({name:"John"})
db.users.insertOne({name:"David", age : 17, address : { street "North Street"} , hobbies = ["Running]})
db.users.insertMany([ {name: "Sally"} , { name : "James"}])

db.users.find()
db.users.find().limit(2)
db.users.find().sort({name: 1})
db.users.find().sort({name: -1}) //reverse order
db.users.find().sort({age : 1, name: -1})
db.users.find({name : "John}) //where query
db.users.find({name : "John} , { name : 1, age : 1 , _id : 0}) //get name and age //dont get id
db.users.find({name : { $eq : "John }})  //complex query
db.users.find({name : { $neq : "John }}) //name not equal to John
db.users.find({name : { $eq : "John } , age : { $eq : 17}}) // AND

$exists: true
$gte, $lte, &gt, &lt
$in
$and[ {name: ""} , { age : 28}]
$or[ {name: ""} , { age : 28}]

MongoDB Self hosted. Download Community Edition
MongoDB Atlas : Database as a service. On AWS, Azure
MongoDB Compas : UI for MongoDB
Realm : For Mobile apps.
Search : Lucene powered search
Online Archive
DataLake