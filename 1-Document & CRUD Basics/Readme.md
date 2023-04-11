## Document & CRUD Basics

#### Start/Stop MongoDB Service

```
net start MongoDB

net stop MongoDB
```

#### Create a database

```
use databaseName

use shop
```

Output:

```
switched to db shop
```

#### List all the databases

```
show dbs
```

Output:

```
admin        40.00 KiB
companyData  56.00 KiB
config       60.00 KiB
local        72.00 KiB
shop         40.00 KiB
```

#### Create/Use a collection

```
db.collectionName

db.products
```

Output:

```
shop.products
```

#### Create/Use a collection

```
db.collectionName

db.products
```

Output:

```
shop.products
```

#### List all the collections

```
db.collectionName

show collections
```

Output:

```
products
```

#### List all the collections

```
db.collectionName

show collections
```

Output:

```
products
```

#### Inserting a document

```
db.products.insertOne({name: "Product One", price: 12.99})

// Insert with Custom ID
db.products.insertOne({_id: 123, name: "Product One", price: 12.99})
```

Output:

```
{
  acknowledged: true,
  insertedId: ObjectId("64351710e3e0c7fe49934ea9")
}
```

#### Inserting a document

```
db.products.insertOne({name: "Product One", price: 12.99})

// Insert with Custom ID
db.products.insertOne({_id: 123, name: "Product Two", price: 10.99})

// Insert many documents
db.products.insertMany([
{name: "Product Three", price: 4.99},
{name: "Product Four", price: 2.99}
])
```

Output:

```
{
  acknowledged: true,
  insertedId: ObjectId("64351710e3e0c7fe49934ea9")
}
```

#### Finding a document

```
// Show all the documents
db.products.find()

// Find the document whose ID is 123
db.products.findOne({_id: 123})

// Find all the documents whose isOutOfStock property is TRUE
db.products.find({isOutOfStock: true})

// Find all the documents whose price property is greater than 5
db.products.find({price: {$gt: 5}})
```

Output:

```
// Show all the documents
[
  {
    _id: ObjectId("64351a36e3e0c7fe49934eaf"),
    name: 'Product One',
    price: 12.99
  },
  { _id: 123, name: 'Product Two', price: 10.99 },
  {
    _id: ObjectId("64351a40e3e0c7fe49934eb0"),
    name: 'Product Three',
    price: 4.99
  },
  {
    _id: ObjectId("64351a40e3e0c7fe49934eb1"),
    name: 'Product Four',
    price: 2.99
  }
]

// Find the document whose ID is 123
{ _id: 123, name: 'Product Two', price: 10.99 }

// Find all the documents whose isOutOfStock property is TRUE


//  Find all the documents whose price property is greater than 5
[
  {
    _id: ObjectId("64351a36e3e0c7fe49934eaf"),
    name: 'Product One',
    price: 12.99
  },
  { _id: 123, name: 'Product Two', price: 10.99 }
]
```
