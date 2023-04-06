* import .json file in mongodb: `mongoimport person.json -d databaseName - collectionName --jsonArray`
* example `mongoimport -d contactData -c contacts persons.json --jsonArray`

* connect mongo `mongosh`
* see databases `show dbs`
* connect database `use contactData`
* see collections `show collections`
* see a single contact `db.contacts.findOne()`
* see contacts that age greater than 60 `db.contacts.find({"dob.age": {$gt: 60}})`
* see contacts count that age greater than 60 `db.contacts.find({"dob.age": {$gt: 60}}).count()`
* see how works mongo db find() `db.contacts.explain().find({"dob.age": {$gt: 60}})`
* see how works more detailed mongo db find() `db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})`


* create index `db.contacts.createIndex({"dob.age": 1})`  can be (-1 desc and 1 asc) 
* 60 see how works more detailed mongo db find() `db.contacts.explain("executionStats").find({"dob.age": {$gt: 60}})`
*  age 20 see how works more detailed mongo db find() `db.contacts.explain("executionStats").find({"dob.age": {$gt: 20}})`
* delete index `db.contacts.dropIndex({"dob.age": 1})`
* note index restriction one if your output is 20% or 30% of the data index will help you if 70% 80% not help it will slow


* see contact `db.contacts.findOne()`
* create index for string type `db.contacts.createIndex({gender: 1})`  can be (-1 desc and 1 asc) 
* see how works more detailed mongo db find() `db.contacts.explain("executionStats").find({gender: "male"})`
* delete index `db.contacts.dropIndex({gender: 1})`

* create compound index `db.contacts.createIndex({"dob.age": 1, gender: 1})`  
*  33 male -> 20age
*  35 female -> 24age
*  37 male -> 30age
*  53 male -> 46age
*  `db.contacts.explain().find({"dob.age": 35, gender: "male"})`
*  `db.contacts.explain().find({"dob.age": 35})`
*  `db.contacts.explain().find({gender: "male"})`

* sort by gender asc `db.contacts.explain().find({"dob.age": 35}).sort({gender: 1})`

* index info: `db.contacts.getIndexes()`
* delete index: `db.contacts.dropIndex({"dob.age": 1, gender: 1})`

### configuring indexes: 
* create unique index for emails `db.contacts.createIndex({email: 1}, {unique: true})`
*  `db.contacts.find({email: "abigail.clark@example.com"}).count()`


*  partial index: `db.contacts.createIndex({"dob.age": 1}, {partialFilterExpression: {gender: "male"}})`
* `db.contacts.find({"dob.age": {$gt: 60} })`
* `db.contacts.explain().find({"dob.age": {$gt: 60}})` see there are female
* `db.contacts.explain().find({"dob.age": {$gt: 60}, gender: "male"})`


* `db.users.insertMany([{name: "Ali", email: "ali@gmail.com"}, {name: "Vali"}])`
* `db.users.createIndex({email: 1}, {unique: true})`
* `db.users.insertOne({name: "Aysu"})`
* `db.users.dropIndex({email: 1})`
* `db.users.createIndex({email: 1}, {unique: true, partialFilterExpression: {email: {$exists: true}}})`
* `db.users.insertOne({name: "Aysu"})`
* `db.users.insertOne({name: "Ayla", email: "ali@gmail.com"})`


### Time to live index
* `db.sessions.insertOne({data: "This is a test data", createdAt: new Date()})`
* `db.sessions.find()`
* `db.sessions.createIndex({createdAt: 1}, {expireAfterSeconds: 10})`