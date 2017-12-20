# Mongodb Assignment
## Assignment 1
* Which of the indexes could be used by MongoDB to assist in answering the query?
The query time was calculated by instructing mongo to use only index given by ```hint()``` and following are the results in a randomly generated data:
1. id
```
db.animals.find( {"firstNumber":{'$lt':10000},"secondNumber":{'$gt':5000}}, {"firstNumber":1,"thirdNumber":1}).sort({"thirdNumber":-1}).hint('_id_').explain();
```
- the query took 148 ms
- this indexing does not help in any of the operations

2. firstNumber_1_secondNumber_1
```
db.animals.find( {"firstNumber":{'$lt':10000},"secondNumber":{'$gt':5000}}, {"firstNumber":1,"thirdNumber":1}).sort({"thirdNumber":-1}).hint('firstNumber_1_secondNumber_1').explain();
```
- the query took 121 ms
- the index helps in the ```find()``` but does not help in ```sort()``` as the result from query is given according to the ```firstNumber```, then only ```thirdNumber``` so the data is not sorted according to the thirdNumber

3. firstNumber_1_thirdNumber_1
```
db.animals.find( {"firstNumber":{'$lt':10000},"secondNumber":{'$gt':5000}}, {"firstNumber":1,"thirdNumber":1}).sort({"thirdNumber":-1}).hint('firstNumber_1_thirdNumber_1').explain();
```
- the query took 127 ms
- the index does help in ```find()``` but not in ```sort()```. Also since it does not have index for secondNumber it is slower than previous indexing

4. thirdNumber_1
```
db.animals.find( {"firstNumber":{'$lt':10000},"secondNumber":{'$gt':5000}}, {"firstNumber":1,"thirdNumber":1}).sort({"thirdNumber":-1}).hint('thirdNumber_1').explain();
```
- the query took only 49 ms fastest of all the indexing
- clearly, it does not help in ```find()``` but helps in ```sort()``` but sort being the more intensive of two operation the query executes much faster than other indexings.

5. firstNumber_1_secondNumber_thirdNumber_-1
```
db.animals.find( {"firstNumber":{'$lt':10000},"secondNumber":{'$gt':5000}}, {"firstNumber":1,"thirdNumber":1}).sort({"thirdNumber":-1}).hint('firstNumber_1_secondNumber_1_thirdNumber_-1').explain();
```
- this only helps in ```find()``` and not in sort because the output result is sorted on the basis of firstNumber and does not help in sorting on the basis of thirdNumber


## Assignment 2
### Which of the following option could potentially improve the speed of inserts?
1. Add an index on last_name, first_name if one does not already exist
- Write operations do not benefit from indexing rather it increases overhead for insert operation

2. Remove all indexes from the collections, leaving only the index on ```_id``` in place
- this will speed up inserts as, the write operation will not have to maintain indexes

3. Provide a hint to MongoDB that it should not use an index for the inserts
- this is irrelevant as inserts do not use indexes 

4. Set w=0, false on writes
- this is unacknowledged write and according to ```write concern``` in mongo documentation, this will certainly increase write speed but risk consistency. 

5. Build a replica set and insert data into the secondary nodes to free up the primary nodes.
- the data is always inserted to primary node then it is replicated across secondary nodes, so making replica does speed up read but does not speed up writes.

## Assignment 3
- The code given above inserts only one instance of ```animals```, after that it exits with following error:
```
E11000 duplicate key error index: test.animals.$_id_ dup key: { : ObjectId('5a39bc95a424d425206298ff') }
```
clearly the remove operation only removes ```animal``` from ```animal``` document so the ```animal``` row is still present in the database, thus, the same key ```animal``` can't have another value ```cat``` and throws error instead.


