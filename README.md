# Structure API DOC

How to use
------------
> POST http://#{host}:#{port}/#{method}/

```python
import requests
url = 'http://127.0.0.1:8000/%s/'

#insert structure
molfile = 'xxxxxx'
r = request.post(
  url = url % 'insert',
  data = {
    molfile: molfile,
    id: 1
  }
)
print r.json()

#sub search
molfile = 'xxxxxx'
r = request.post(
  url = url % 'sub',
  data = {
    molfile: molfile,
    page: 1
  }
})
print r.json()
```

> You can get the molfile from structure editor or parse from smiles or standard naming of chemical

> Parse smiles can use the api - s2m

> Parse standard naming of chemical can use the api - n2m

Method List
------------

#### insert
> Insert product structure to api
* molfile - str, require, product structure
* id - int, require, primary key in your database
* return
```json
//Correct
{"status": true}
//Error
{"status": false, "errcode": 1 | 3}
```

#### update
> Update product structure data
* molfile - str, require, product structure
* id - int, require, primary key in your database
* return
```json
//Correct
{"status": true}
//Error
{"status": false, "errcode": 2 | 3}
```

#### delete
> Delete product data
* id - int, require, primary key in your database
```json
//Correct
{"status": true}
//Error
{"status": false, "errcode": 2}
```

#### sub
> Structure sub search
* molfile - str, require, product structure
* page - int, require
```json
//Correct
{
  "status": true, 
  "rows": [{
    "id": "int",
    "molwt": "float", 
    "formula": "str"
  }],
  "sub": "str",
  "has_next": true | false
}
//Error
{"status": false, "errcode": 3}
```

#### sim
> Structure similarity search
* molfile - str, require, product structure
* sim - float, require, Range: 0.5 to 0.9
* page - int, require
```json
//Correct
{
  "status": true, 
  "rows": [{
    "id": "int",
    "molwt": "float", 
    "formula": "str"
  }],
  "sub": "str",
  "has_next": true | false
}
//Error
{"status": false, "errcode": 3}
```

#### exact
* molfile - str, require, product structure
* page - int, require
```json
//Correct
{
  "status": true, 
  "rows": [{
    "id": "int",
    "molwt": "float", 
    "formula": "str"
  }],
  "sub": "str",
  "has_next": true | false
}
//Error
{"status": false, "errcode": 3}
```

#### n2m
> Name to molfile
* name - str, require, standard naming of chemical
```json
//Correct
{"status": true, "molfile": "str" | false}
{"status": false, "errcode": 3}
```

#### getImage
> Get structure image binary data
* id - int, require, primary key in your database
* sub - str, optional, highlight structur in image
> If the response Content-Type is image/png then it is correct, you can output the response data
```json
//Error
{"status": false, "errcode": 2 | 3}
```

#### molwt
> Get molecular weight of product
* id - int, require, primary key in your database
```json
//Correct
{"status": true, "molwt": "float"}
//Error
{"status": false, "errcode": 2}
```

#### formula
> Get molecular formula of product
* id - int, require, primary key in your database
```json
//Correct
{"status": true, "formula": "str"}
//Error
{"status": false, "errcode": 2}
```

#### m2s
> Molfile to smiles
* molfile - str, require, product structure
```json
//Correct
{"status": true, "smiles": "str"}
//Error
{"status": false, "errcode": 3}
```

#### s2m
> Smiles to molfile
* smiles - str, require, product structure
```json
//Correct
{"status": true, "molfile": "str"}
//Error
{"status": false, "errcode": 3}
```

#### smi2img
> Get image data from smiles
* smiles - str, require, product structure
> If the response Content-Type is image/png then it is correct, you can output the response data
```json
//Error
{"status": false, "errcode": 3}
```

Error Code Description
------------------------
* 1 - The id is exists
* 2 - The id isn't exists
* 3 - The parameters error, you can get the info from the response json

How to sync with my database
-------------------------------
> You can use the `insert` api when you create a new product, remeber use sql transaction.

> Delete, update is same as insert action.

***Exam***

1. Transaction Start
1. Sql - insert into table(...) values (...)
1. Get the id for this product
1. Use `insert` api
1. if api success transaction commit else transaction rollback
