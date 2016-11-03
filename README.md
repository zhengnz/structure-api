# Structure API DOC

How to use
------------
> POST http://#{host}:#{port}/#{method}/

Method List
------------

#### insert
> Insert product structure to api
* molfile - str, require, product structure
* id - int, require, primary key in your database
* return
```json
//Correct
{status: true}
//Error
{status: false, errcode: 1 | 3}
```

#### update
