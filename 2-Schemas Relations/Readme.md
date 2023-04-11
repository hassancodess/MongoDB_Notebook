## Relations

#### One to One using Embedded Document

```
use hospitalDB

db.patients.insertOne({
  name: 'Saqib',
  age: 15,
  diseaseSummary: {
    diseases: ['cold', 'broken leg'],
  },
})
```

#### One to One using References

```
use hospitalDB

db.patients.insertOne({
  name: 'Saqib',
  age: 15,
  diseases: ['d1', 'd2'],
})


db.diseases.insertMany(
  { _id: 'd1', name: 'cold' },
  { _id: 'd2', name: 'broken leg' }
)
```

#### One to Many using Embedded Document

```
db.questionThreads.insertOne({
  creator: 'Max',
  question: 'How does that work?',
  answers: [{ text: 'It works like that' }, { text: 'Thanks!' }],
})
```

#### One to Many using References

```
use cityData

db.cities.insertMany([
  { _id: 1, name: 'Rawalpindi', coordinates: { lat: 51, lng: 55 } },
  { _id: 2, name: 'Chakwal', coordinates: { lat: 32, lng: 51 } },
  { _id: 3, name: 'Mandi Bahauddin', coordinates: { lat: 32, lng: 51 } },
])

db.cities.findOne()

db.citizens.insertMany([
  { name: 'Hassan', cityId: 1 },
  { name: 'Sohaib', cityId: 2 },
  { name: 'Irfan', cityId: 3 },
])
```

#### Many to Many using Embedded Document

```
use bookRegistry

db.books.insertOne({
  name: 'Indistractable',
  authors: [
    { name: 'Nir Eyal', age: 30 },
    { name: 'Julie Li', age: 28 },
  ],
})
```

#### Many to Many using References

```
use bookRegistry

db.authors.insertMany([
  { _id: 1, name: 'Nir Eyal', age: 30, city: 'NYC' },
  { _id: 2, name: 'Julie Li', age: 30, city: 'NYC' },
])

db.books.insertOne({ name: 'Indistractable', authors: [1, 2] })
```

## Merging Relations

### $lookup

```
db.books.aggregate([
  {
    $lookup: {
      from: 'authors',
      localField: 'authors',
      foreignField: '_id',
      as "creators"
    },
  },
])
```

### Output

```
[
  {
    _id: ObjectId("6435888137a2562dd70fecfc"),
    name: 'Indistractable',
    authors: [ 1, 2 ],
    creators: [
      { _id: 2, name: 'Julie Li', age: 30, city: 'NYC' },
      { _id: 1, name: 'Nir Eyal', age: 30, city: 'NYC' }
    ]
  }
]
```

## Schema Validation

### Adding Validation

```
db.createCollection('posts', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        creator: {
          bsonType: 'objecId',
          description: 'must be an objectId and is required',
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'text',
                description: 'must be a string and is required',
              },
              author: {
                bsonType: 'objecId',
                description: 'must be an objectId and is required',
              },
            },
          },
        },
      },
    },
  },
})

```

### Changing Validation

```

db.runCommand({
  collMod: 'posts',
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required',
        },
        creator: {
          bsonType: 'objecId',
          description: 'must be an objectId and is required',
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'text',
                description: 'must be a string and is required',
              },
              author: {
                bsonType: 'objecId',
                description: 'must be an objectId and is required',
              },
            },
          },
        },
      },
    },
  },
  validationAction: 'warn',
})

```
