# assignment_practice_with_mongodb


Querying with the MongoDB shell


## Getting Started

**IMPORTANT** don't run `__seeds__.js`. It is there only to generate `__products__.js` in the case that it **MUST** be regenerated. Regenerating that data will make query results different across instances of this assignment.


## Products

To get started import `__products__.js` into your MongoDB database with the following command:

```bash
$ mongoimport --db test --collection products --file __products__.js
```


## Restaurants

Restaurant data is imported from the MongoDB test database provide [here](https://docs.mongodb.com/getting-started/shell/import-data/).

Import the data from the `__restaurants__.js` file.

```bash
$ mongoimport --db test --collection restaurants --file __restaurants__.js
```

UPDATING Products:
	1. db.products.update({department: 'Hardware'}, {$set: {department: 'Hardware Tools'}}, {multi: true});
	2. db.products.update({department: 'Hardware Tools'}, {$max: {sales: 50}}, {multi: true);
	3. db.products.update({department: 'Hardware Tools'}, {$inc: {price: 10}}, {multi: true});
	4. db.products.update({department: 'Hardware Tools'}, {$set: {department: 'Hardware'}}, {multi: true});
	5. db.products.update({department: 'Hardware'}, {$inc: {price: -10}}, {multi: true});
	6. db.products.update({department: 'Hardware'}, {$min: {sales: 10}}, {multi: true});
	7. db.products.update({department: 'Hardware'}, {$inc: {sales: 1}});

REMOVING Products:
	1. db.products.remove({department: 'Hardware'}, {justOne: true});
	2. db.products.remove({department: 'Hardware'});

FINDING Products:
  1. db.products.find({stock: 0},{'name': true, id: 0});
  2. db.products.find({price: {$lt: 100}}, {'stock':1,'id':0});
  3. db.products.find({$and: [{price: {$lt: 1000}}, {price: {$gt: 100}}]}, {'name':1,'stock':1,'color':1,'department':1,id:0});
  4. db.products.find({color: 'red'},{'name':1, id:0});
  5. db.products.find({$or: [{color:'red'}, {color: 'blue'}]}, {_id:1});
  6. db.products.find({$and: [{color: {$ne: 'blue'}}, {color: {$ne: 'red'}}]}, {'name':1, id:0});
  7. db.products.find({$and: [{department: {$ne: 'Sports'}}, {departments: {$ne: 'Games'}}]}, {'name':1, id:0});
