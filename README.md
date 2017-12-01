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
  8. db.products.find({name: /^f.+s$/i}, {'name': 1, 'price': 1, id: 0});
  9. db.products.find({ $where: 'this.name.toString()[0] === "T"' }, {'name':1, id:0});
  10. db.products.find({ $where: 'this.name.toString()[0] === "F" || this.name.toString()[this.name.toString().length - 1] === "s"'}, {'name': 1, id: 0});
  11. db.products.find({ $where: 'this.name.toString()[0] === "T" && this.price < 100'}, { 'name': 1, id:0});
  12. db.products.find({ $where: '(this.name.toString()[0] === "A" && this.price >= 100) || (this.name.toString()[0] === "B" && this.price <= 100)'}, {'name':1, 'price':1, id: 0});

AGGREGATING Products:
  1. db.products.aggregate( [{$match:{}}, {$group: {_id: '$department', total: {$sum: '$sales'}}}, {$sort: {_id: 1}}] )
  2. db.products.aggregate( [{$match:{price:{$gte: 100}}}, {$group: {_id: '$department', total: {$sum: '$sales'}}}, {$sort: {_id: 1}}] );
  3. db.products.aggregate( [{$match:{stock:{$eq: 0}}}, {$group: {_id: '$department', total: {$sum: 1}}}, {$sort: {_id: 1}}] );

MAP-REDUCE Products:
  1. db.products.mapReduce( function() { emit(this.color, this.color); }, function(keys, values) { return values.length; },{ query: {}, out: 'color'} ).find();
  2. db.products.mapReduce( function() {emit(this.department, this.sales);}, function(keys, values) { return Array.sum(values)}, { query: {}, out: "department_totals"}).find();
  3. db.products.mapReduce( function() { emit(this.name, this.stock*this.price)}, function(keys, values) { return Array.sum(values)}, { query: {}, 'out': "potential_sales"}).find();
  4. db.products.mapReduce( function() {emit(this.name, this.sales*this.price+this.stock*this.price)}, function(key, values) {return Array.sum(values)}, {query: {}, 'out': "total & potential revenue"}).find();

SINGLE PURPOSE AGGREGATION
  1. db.products.count();
  2. db.products.count({stock: 0});
  3. db.products.count({stock: 100});
  4. db.products.count({stock: {$lte: 5}});
  5. db.products.distinct('department');
  6. db.products.distinct('color');
  7. db.products.group({key: {department: 1}, cond: {stock: {$eq: 0}}, reduce: function(cur, result){result.count += 1}, initial: {count: 0}});

MONGO DB RESTAURANTS DATABASE
  1. db.restaurants.find({},{'name':1, _id:0}).limit(5);
  2. db.restaurants.find({$or: [{'grades.grade': 'A'}, {'grades.grade': 'B'}]},{'name':1, _id:0});
  3. db.restaurants.find({'grades.score': {$gt: 20}},{'name':1, _id:0});
  4. db.restaurants.group({key: {cuisine: 1}, cond: {borough: {$eq: 'Bronx'}}, reduce: function(cur, result){result.name=cur.name}, initial: {name: ''}});
