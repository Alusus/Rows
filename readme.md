# Rows
[[عربي]](readme.ar.md)

An ORM (object relational mapping) library for Alusus.

## Adding to the Project

We can install this library using the following statements:

```
import "Apm";
Apm.importFile("Alusus/Rows", { "Rows.alusus", "<driver>" });
```

A driver name must be included in the second argument, unless if you are implementing your own DB
driver. The driver lets you connect with a specific DB type. Rows does not automatically load all
drivers because these drivers have system dependencies and including all drivers automatically
forces the user to install system dependencies that he might not need.

### PostgreSQL

```
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Postgresql.alusus" });
use Rows;
def db: Db(PostgresqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
```

### MySql

```
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Rows;
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
```

### Sqlite

```
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Sqlite.alusus" });
use Rows;
def db: Db(SqliteDriver(ConnectionParams().{
    dbName = "alusus.db";
}));
```

## Example

### Accessing the DB Manually

```
import "Apm";
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Srl;
use Rows;

// a variable for the database that holds its information
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));

// check if the connection is established or not
if !db.driver.isConnected() {
    System.fail(1, String("Error connecting to DB: ") + db.getLastError());
}

// create the model
if !!db.exec(CreateTable().{
    name = String("users");  // table name
    notExists = true;  // create it only if is not existed
    // columns information
    columns = Map[String, SrdRef[Column]]().{
        set(String("id"), Column().{
            dataType = Integer();
            notNull = true;
            unique = true;
        });
        set(String("name"), Column().{
            dataType = VarChar(90);
            notNull = true;
            unique = true;
            default = String("me");
        });
        set(String("address"), Column().{
            dataType = VarChar(200);
        });
    };
    primaryKey = String("id");  // primary key
}) {
    System.fail(1, String("Query failed: ") + db.getLastError());
}

// create another model
// this model is linked to the first one by a foreign key
def res: Possible[Int] = db.exec(CreateTable().{
    name = String("roles");
    notExists = true;
    columns = Map[String, SrdRef[Column]]().{
        set(String("id"), Column().{
            dataType = Integer();
            notNull = true;
            unique = true;
        });
        set(String("name"), Column().{
            dataType = VarChar(90);
            notNull = true;
            unique = true;
            default = String("viewer");
        });
        set(String("userId"), Column().{
            dataType = Integer();
        });
    };
    primaryKey = String("id");
    foreignKeys = Array[SrdRef[ForeignKey]]({
        ForeignKey(String("userId"), String("users"), String("id"))
    });
});
if not res {
    System.fail(1, String("Query failed: ") + res.error.getMessage());
}

// Insert some data

db.exec("delete from users");

def names: Array[String]({ String("Sarmad"), String("Sleman"), String("Abdullah"), String("Salem") });
def addresses: Array[String]({ String("Canada"), String("Syria"), String("KSA"), String("KSA") });

def i: Int;
for i = 0, i < names.getLength(), ++i {
    def res: Possible[Int] = db.exec(Insert().{
        table = String("users");
        columns = Array[String]({ String("id"), String("name"), String("address") });
        data = Array[String]({ String("15") + i, names(i), addresses(i)  });
    });
    if !!res {
        System.fail(1, String("Query failed: ") + res.error.getMessage());
    }
}

// Read the data

def data: Array[Array[String]];
def res: Possible[Array[Array[String]]];
res = db.exec(Select().{
    table = String("users");
});
if !!res {
    System.fail(1, String("Query failed: ") + res.error.getMessage());
}
data = res;

def i: int = 0;
def j: int = 0;
for i = 0, i < data.getLength(), i = i + 1 {
    for j = 0, j < data(i).getLength(), j = j + 1 {
        Console.print(data(i)(j));
        Console.print("    ");
    }
    Console.print("\n");
}
```

### Using ORM

```
import "Apm";
Apm.importFile("Alusus/Rows", { "Rows.alusus", "Drivers/Mysql.alusus" });
use Srl;
use Rows;

// a modifier that define a model with the name cars
@model["cars"]
class Car {
    define_model_essentials[];

    // id column
    @notNull  // modifier to ensure the value is not null
    @primaryKey  // primary key modifier
    @Integer  // integer value column modifier
    @column  // column modifier, to make what follows as column
    def id: Int;

    // car name column
    @VarChar["50"]  // variable length char column value modifier with max length == 50
    @defult["My Car"]  // default value modifier, if no value provided
    @column
    def name: String;

    @Float  // float column value modifier
    @column
    def price: Float;
}

// connect to the database
def db: Db(MysqlDriver(ConnectionParams().{
    dbName = "alusus";
    userName = "alusus";
    password = "alusus";
    host = "0.0.0.0";
}));
if !db.isConnected() {
    System.fail(1, String("Error connecting to DB: ") + db.getLastError());
}

// call schemaBuilder on the previous object to build the model
// based what we specified with the modifiers
db.schemaBuilder[Car].create();

// delete any previous rows in the model (from a previous execution of the code)
db.from[Car].delete();

def i: int=1
for i = 1, i < 30, i = i+1 {
    def c: Car;
    c.id = i
    c.name = String("Car ") + i;
    c.price = 200.0 * i
    // after we give the values for the object, call save to add it as new row.
    db.save[Car](c);
}
Console.print("\n");

// print the rows of the model to ensure everything as expected.
func printRows (r: Array[SrdRef[Car]]) {
    def k: int=0;
    for k = 0, k < r.getLength(), k = k+1 {
        Console.print(r(k).id);
        Console.print("\t\t");
        Console.print(r(k).name);
        Console.print("\t\t");
        Console.print(r(k).price);
        Console.print("\n");
    }
}
```

## Object Relational Mapping

Rows library provides an ORM functionality by allowing DB talbes to be defined as Alusus classes
and automatically mapping class objects to table rows for reading and writing to the DB. To enable
this the user needs to make the following additions to the classes:

* Add the modifier `@model` and specify the table name as a modifier arg.
* Add `define_model_essentials[]` to the beginning of the class.
* Add `@column` to each variable that maps to a table field and specify the field name as modifier arg.
* Add DB type modifiers to the variables.
* Add modifiers related to indexing, default values, and required fields, if needed.

Example

```
@model["cars"]
class Car {
    define_model_essentials[];

    @notNull
    @primaryKey
    @Integer
    @column
    def id: int;

    @VarChar["50"]
    @defult["My Car"]
    @column
    def name: String;

    @Float
    @column["the_price"]
    def price: Float;

    @VarChar["50"]
    @column
    def fullName: Nullable[String];
}
```

After making these changes to the class then class will be useable in functions like `Db.from`
or `Db.save` or `SchemaBuilder.create`.

### Tables and Columns Modifiers

* `@model`
* `@column`
* `@primaryKey`
* `@foreignKey`
* `@notNull`
* `@unique`
* `@default`: For specifying field default values.
* `@check`: For specifying DB conditions for table fields.

### Data Type Modifiers

* `@Integer`
* `@BigInteger`
* `@SmallInteger`
* `@TinyInteger`
* `@Boolean`
* `@Real`
* `@Float`
* `@Decimal`
* `@Xml`
* `@VarChar`
* `@CharType`
* `@Text`
* `@Date`
* `@DateTime`

## Functions and Types

### ConnectionParams class

```
class ConnectionParams {
    def dbName: String = "";
    def userName: String = "";
    def password: String = "";
    def host: String = "";
    def port: int = 0;
}
```

This class contains the required information to connect to the database

`dbName` the name of the database we want to connect to it.

`userName` the username for our account in the database.

`password` the password of our account in the database

`host` the host address that run the database.

`port` the port on the host that we can access database through it.

### CreateTable class

```
class CreateTable {
    handler this.name = String;
    handler this.notExists = Bool;
    handler this.columns = Map[String, SrdRef[Column]];
    handler this.primaryKey = Array[String];
    handler this.foreignKeys = Array[SrdRef[ForeignKey]];
}
```

This class is used to create a model with a given information.

`name` the model name.

`notExists` specify if we need to create the model only of its not already exist.

`columns` a map between column name and its information.

`primaryKey` the primary key of the model.

`foreignKeys` the foreign keys of the model.

### Delete class

```
class Delete {
    handler this.table = String;
    handler this.condition(statement: CharsPtr, args: ...any);
}
```

This class is used to delete rows from a model based on a condition.

`table` the model name.

`condition` the condition the a row must satisfy in order to delete it.

### Value class

```
class Value {
    handler this~init();
    handler this~init(Bool);
    handler this~init(Nullable[Bool]);
    handler this~init(Int[64]);
    handler this~init(Nullable[Int[32]]);
    handler this~init(Nullable[Int[64]]);
    handler this~init(Float[64]);
    handler this~init(Nullable[Float[32]]);
    handler this~init(Nullable[Float[64]]);
    handler this~init(String);
    handler this~init(Nullable[String]);
    handler this~init(CharsPtr);
}
```

This class is used to pass values of any type to the `Insert` and `Update` operations.

### Insert class

```
class Insert {
    handler this.table = String;
    handler this.data = Array[Value];
    handler this.columns = Array[String];
}
```

This class is used to insert a row to the model.

`table` the model name.

`data` the values of the row we want to add.

`columns` the names of the columns that corresponds to the values.


### Select class

```
class Select {
    handler this.table = Array[String];
    handler this.fields = Array[String];
    handler this.condition(statement: CharsPtr, args: ...any);
    handler this.orderBy = Array[String];
}
```

This class is used to retrieve rows from the model.

`table` the model name.

`columns` the names of the columns we want to retrieve.

`condition` the condition the a row must satisfy in order to select it.

`orderBy` the order of the selected rows.

### Update class

```
class Update {
    handler this.table = String;
    handler this.data = Array[Value];
    handler this.columns = Array[String];
    handler this.condition(statement: CharsPtr, args: ...any);
}
```

This class is used to update a row, or a set of rows in a model.

`table` the model name.

`data` the new values of the columns

`columns` the columns we want to update its values.

`condition` the condition the a row must satisfy in order to update it.

### Column class

```
class Column {
    handler this.dataType = SrdRef[DataType];
    handler this.notNull = Bool;
    handler this.unique = Bool;
    handler this.default = String;
    handler this.check(statement: CharsPtr, args: ...any);
}
```

This class holds the column information.

`dataType` the type of data we want to store in the column.

`notNull` specify if this column value is required, or it can be null.

`unique` specify if this column values are unique or not.

`default` the default value for this column when no value is provided.

`check` the check to apply on the value before insert it.

### Query class

```
class Query [Model: type] {
    handler [exp: ast] this.order:ref[this_type];
    @member macro where [this, condition];
    @member macro update [this, expression];
    handler this.select(): Possible[Array[SrdRef[Model]]];
    handler this.save(model: ref[Model]): Possible[Int];
    handler this.delete(model: ref[Model]): Possible[Int];
}
```

`order` specify the order of the query.

`where` a macro to set the condition of the query.

parameters:

* `this` a pointer to an Query object.
* `condition` the condition we want to set.

`update` a macro to apply an update query on the model.

parameters:

* `this` a pointer to an Query object.
* `expression` the update expression.

`select` used to retrieve rows from a model.

`save` used to save a row in the model.

parameters:

* `model` a reference to the model we want to save the row to it.

`delete` used to delete from the model.

parameters:

* `model` a reference to the model we want to delete the row from it.

This class is used to combine the clauses of an operation before execution. Usually the user does
not need to instantiate this manually; instead, it's instantiated through the `Db.from` method.
For example:

```
db.from[User].where[name = arg1].update[address = arg2];
```

### SchemaBuilder class

```
class SchemaBuilder [Model: type] {
    handler this.create(): Possible[Int];
}
```

This class is used as a template to create the table. The template arg must be a class with the
correct decorations as specified in the Object Relational Mapping section above.

`create` Creates the DB table from the provided class.

### Db class

```
class Db {
    handler this.init(d: SrdRef[Driver]);
    handler this.isConnected(): Bool;
    handler this.getLastError(): String;
    handler this.exec(select: ref[Select]): Possible[Array[Array[String]]];
    handler this.exec(update: ref[Update]): Possible[Int];
    handler this.exec(insert: ref[Insert]): Possible[Int];
    handler this.exec(delete: ref[Delete]): Possible[Int];
    handler this.exec(createTable: ref[CreateTable]): Possible[Int];
    handler this.exec(statement: CharsPtr, args: ...any): Possible[Int];
    handler [Model: type] this.from: Query[Model];
    handler [Model: type] this.save(model: ref[Model]);
    handler [Model: type] this.schemaBuilder: SchemaBuilder[Model];
}
```

This class is used to manage the access to the database and executing many queries on it.

`init` used to initialize the database with the given driver.

`isConnected` used to check if there is a connection with the database.

`getLastError` used to get the last error.

`exec` used to execute a query. There is an overload from this function for each type of queries in
addition to an overload for executing raw SQL statements. The version for raw SQL splits the SQL
structure from the data, which are passed as extra args to the function similar to printf. The
following types of data params are supported by this function:
* String: %s
* CharsPtr: %p
* Int: %i
* Int[64]: %l
* Float: %f
* Float[64]: %d
* Array[String]: %as
* Array[CharsPtr]: %ap
* Array[Int]: %ai
* Array[Int[64]]: %al
* Array[Float]: %af
* Array[Float[64]]: %ad

`from` used to return a query based on the information of this class.

`save` used to call the method `save` in the class `Query`.

`schemaBuilder` used to return a schemaBuilder based on the information of this class.

### getBuildDependencies Function

```
func getBuildDependencies(): Array[String];
```

Each of the available DB drivers defines this function for getting the external build dependencies
needed to build an executable that connects with the given database type.

### Errors

The `Errors` submodule contains error codes for all errors that can be returned by this library.

* `Errors.DB_NOT_INITIALIZED`: Raised when the `Db` class is being used before being initialized.
* `Errors.DB_NOT_CONNECTED`: Raised when the `Db` is initialized with a DB driver but the driver is not properly
  connected.
* `Errors.SQL_ERROR`: Raised when an error occurs during the execution of an SQL statement.
* `Errors.CONNECTION_MISSING`: Raised when `save` or `delete` is called on a model that isn't connected to a `Db`
  object, i.e. it wasn't previously loaded using a `Db` object.

## To Do
- [ ] Add remaining DB operations:
    - [ ] Adding column.
    - [ ] Removing colum.
    - [ ] Renaming column.
    - [ ] Changing column type.
    - [ ] Adding index.
    - [ ] Removing index.
    - [ ] Removing table.
    - [ ] DB migrations.
- [ ] Add support for relational operations:
    - [ ] Includes (eager loading).
    - [ ] Loading related records.
    - [ ] Associating and unassociating records.
- [ ] Add support for async operations.
- [ ] Add support for error codes for DB errors.
