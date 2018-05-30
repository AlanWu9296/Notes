# Basic Utility 

```bash
sudo service mongod start # start local server of the database
sudo service mongod stop
sudo service mongod restart

mongo #start mongo shell
```

# Mongo Shell

```bash
mongo help
show dbs
show collections
use <database name>
db.<collection>.<method>
.insert()
.find()
.update()
.remove()
```



# Mongoose

```js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');
var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));

db.once('open', function() {
var catSchema = new mongoose.Schema({
    name:String,
    age:Number,
    temperament:String
});
const Cat = mongoose.model('Cat',catSchema);
const kitty = new Cat({ name: 'Zildjian' });
kitty.save().then(() => console.log('meow'));
}
```

```js
// NOTE: methods must be added to the schema before compiling it with mongoose.model()
kittySchema.methods.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}

var Kitten = mongoose.model('Kitten', kittySchema);
var fluffy = new Kitten({ name: 'fluffy' });
fluffy.speak(); // "Meow name is fluffy"
```

```js
 fluffy.save(function (err, fluffy) {
    if (err) return console.error(err);
    fluffy.speak();
  });
```

## schema

Everything in Mongoose starts with a Schema. Each schema maps to a MongoDB collection and defines the shape of the documents within that collection.

```js
  var mongoose = require('mongoose');
  var Schema = mongoose.Schema;

  var blogSchema = new Schema({
    title:  String,
    author: String,
    body:   String,
    comments: [{ body: String, date: Date }],
    date: { type: Date, default: Date.now },
    hidden: Boolean,
    meta: {
      votes: Number,
      favs:  Number
    }
  });
```

The permitted SchemaTypes are:

- String
- Number
- Date
- Buffer
- Boolean
- Mixed
- ObjectId
- Array
- Decimal128
- Map

## model

To use our schema definition, we need to convert our `blogSchema` into a [Model](http://mongoosejs.com/docs/models.html) we can work with. To do so, we pass it into `mongoose.model(modelName, schema)`:

```js
  var Blog = mongoose.model('Blog', blogSchema);
```

1. **querying**:

Finding documents is easy with Mongoose, which supports the [rich](http://www.mongodb.org/display/DOCS/Advanced+Queries) query syntax of MongoDB. Documents can be retreived using each `models` [find](http://mongoosejs.com/docs/api.html#model_Model.find), [findById](http://mongoosejs.com/docs/api.html#model_Model.findById), [findOne](http://mongoosejs.com/docs/api.html#model_Model.findOne), or [where](http://mongoosejs.com/docs/api.html#model_Model.where) static methods.

```js
Tank.find({ size: 'small' }).where('createdDate').gt(oneYearAgo).exec(callback);
```

Mongoose [models](http://mongoosejs.com/docs/models.html) provide several static helper functions for [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete). Each of these functions returns a [mongoose `Query` object](http://mongoosejs.com/docs/api.html#Query).

- [`Model.deleteMany()`](http://mongoosejs.com/docs/api.html#deletemany_deleteMany)
- [`Model.deleteOne()`](http://mongoosejs.com/docs/api.html#deleteone_deleteOne)
- [`Model.find()`](http://mongoosejs.com/docs/api.html#find_find)
- [`Model.findById()`](http://mongoosejs.com/docs/api.html#findbyid_findById)
- [`Model.findByIdAndDelete()`](http://mongoosejs.com/docs/api.html#findbyidanddelete_findByIdAndDelete)
- [`Model.findByIdAndRemove()`](http://mongoosejs.com/docs/api.html#findbyidandremove_findByIdAndRemove)
- [`Model.findByIdAndUpdate()`](http://mongoosejs.com/docs/api.html#findbyidandupdate_findByIdAndUpdate)
- [`Model.findOne()`](http://mongoosejs.com/docs/api.html#findone_findOne)
- [`Model.findOneAndDelete()`](http://mongoosejs.com/docs/api.html#findoneanddelete_findOneAndDelete)
- [`Model.findOneAndRemove()`](http://mongoosejs.com/docs/api.html#findoneandremove_findOneAndRemove)
- [`Model.findOneAndUpdate()`](http://mongoosejs.com/docs/api.html#findoneandremove_findOneAndRemove)
- [`Model.replaceOne()`](http://mongoosejs.com/docs/api.html#replaceone_replaceOne)
- [`Model.updateMany()`](http://mongoosejs.com/docs/api.html#updatemany_updateMany)
- [`Model.updateOne()`](http://mongoosejs.com/docs/api.html#updateone_updateOne)

A mongoose query can be executed in one of two ways. First, if you pass in a `callback` function the operation will be executed immediately with the results passed to the callback.

A query also has a `.then()` function, and thus can be used as a promise.



2. **Deleting**:Models have static `deleteOne()` and `deleteMany()` functions for removing all documents matching the given `filter`.

```js
Tank.deleteOne({ size: 'large' }, function (err) {
  if (err) return handleError(err);
  // deleted at most one tank document
});
```

3. **Updating**: Each `model` has its own `update` method for modifying documents in the database without returning them to your application. See the [API](http://mongoosejs.com/docs/api.html#model_Model.updateOne) docs for more detail.

```js
Tank.updateOne({ size: 'large' }, { name: 'T-90' }, function(err, res) {
  // Updated at most one doc, `res.modifiedCount` contains the number
  // of docs that MongoDB updated
});
```



## instance methods

Instances of `Models` are [documents](http://mongoosejs.com/docs/documents.html). Documents have many of their own [built-in instance methods](http://mongoosejs.com/docs/api.html#document-js). We may also define our own custom document instance methods too.

```js
  // define a schema
  var animalSchema = new Schema({ name: String, type: String });

  // assign a function to the "methods" object of our animalSchema
  animalSchema.methods.findSimilarTypes = function(cb) {
    return this.model('Animal').find({ type: this.type }, cb);
  };
  var Animal = mongoose.model('Animal', animalSchema);
  var dog = new Animal({ type: 'dog' });

  dog.findSimilarTypes(function(err, dogs) {
    console.log(dogs); // woof
  });
```

## static methods

```js
  var Animal = mongoose.model('Animal', animalSchema);
  Animal.findByName('fido', function(err, animals) {
    console.log(animals);
  });
```

## virtuals

[Virtuals](http://mongoosejs.com/docs/api.html#schema_Schema-virtual) are document properties that you can get and set but that do not get persisted to MongoDB. The getters are useful for formatting or combining fields, while setters are useful for de-composing a single value into multiple values for storage.

```js
 // define a schema
  var personSchema = new Schema({
    name: {
      first: String,
      last: String
    }
  });

  // compile our model
  var Person = mongoose.model('Person', personSchema);

  // create a document
  var axl = new Person({
    name: { first: 'Axl', last: 'Rose' }
  });

personSchema.virtual('fullName').
  get(function() { return this.name.first + ' ' + this.name.last; }).
  set(function(v) {
    this.name.first = v.substr(0, v.indexOf(' '));
    this.name.last = v.substr(v.indexOf(' ') + 1);
  });

axl.fullName = 'William Rose'; // Now `axl.name.first` is "William"
```

## Validation

- Validation is defined in the [SchemaType](http://mongoosejs.com/docs/schematypes.html)
- Validation is [middleware](http://mongoosejs.com/docs/middleware.html). Mongoose registers validation as a `pre('save')` hook on every schema by default.
- You can manually run validation using `doc.validate(callback)` or `doc.validateSync()`
- Validators are not run on undefined values. The only exception is the [`required` validator](http://mongoosejs.com/docs/api.html#schematype_SchemaType-required).
- Validation is asynchronously recursive; when you call [Model#save](http://mongoosejs.com/docs/api.html#model_Model-save), sub-document validation is executed as well. If an error occurs, your [Model#save](http://mongoosejs.com/docs/api.html#model_Model-save) callback receives it
- Validation is customizable

**Built-in Validators**

- All [SchemaTypes](http://mongoosejs.com/docs/schematypes.html) have the built-in [required](http://mongoosejs.com/docs/api.html#schematype_SchemaType-required) validator. The required validator uses the [SchemaType's `checkRequired()` function](http://mongoosejs.com/docs/api.html#schematype_SchemaType-checkRequired) to determine if the value satisfies the required validator.
- [Numbers](http://mongoosejs.com/docs/api.html#schema-number-js) have [min](http://mongoosejs.com/docs/api.html#schema_number_SchemaNumber-min) and [max](http://mongoosejs.com/docs/api.html#schema_number_SchemaNumber-max) validators.
- [Strings](http://mongoosejs.com/docs/api.html#schema-string-js) have [enum](http://mongoosejs.com/docs/api.html#schema_string_SchemaString-enum), [match](http://mongoosejs.com/docs/api.html#schema_string_SchemaString-match), [maxlength](http://mongoosejs.com/docs/api.html#schema_string_SchemaString-maxlength) and [minlength](http://mongoosejs.com/docs/api.html#schema_string_SchemaString-minlength) validators.
- **unique** option is not a validator but can still be used like it is

```js
var breakfastSchema = new Schema({
  eggs: {
    type: Number,
    min: [6, 'Too few eggs'],
    max: 12
  },
  bacon: {
    type: Number,
    required: [true, 'Why no bacon?']
  },
  drink: {
    type: String,
    enum: ['Coffee', 'Tea'],
    required: function() {
      return this.bacon > 3;
    }
  }
});
```

**Custom Validators**

Custom validation is declared by passing a validation function. You can find detailed instructions on how to do this in the [`SchemaType#validate()` API docs](http://mongoosejs.com/docs/api.html#schematype_SchemaType-validate).

```js
var userSchema = new Schema({
  phone: {
    type: String,
    validate: {
      validator: function(v) {
        return /\d{3}-\d{3}-\d{4}/.test(v);
      },
      message: '{VALUE} is not a valid phone number!'
    },
    required: [true, 'User phone number required']
  }
});
```

**Async Custom Validators**

```js
var userSchema = new Schema({
  name: {
    type: String,
    // You can also make a validator async by returning a promise. If you
    // return a promise, do **not** specify the `isAsync` option.
    validate: function(v) {
      return new Promise(function(resolve, reject) {
        setTimeout(function() {
          resolve(false);
        }, 5);
      });
```

