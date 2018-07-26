# Intro to Ruby on Rails

Useful Resources
[SQLite](http://www.sqlite.org)
[Active Record Basics](http://guides.rubyonrails.org/active_record_basics.html)

---

1. Homework Review
2. What is SQL?
3. CRUD
4. Create a New App
5. rails dbconsole
6. SQL queries
	* all tables
	* Read (Select)
	* Modes
	* More Read-ing (Select-ing)
	* Select Functions
	* Update (Update)
	* Create (Insert)
	* Delete (Delete)
7. Active Record Intro
8. Rails Console
9. Active Record
	* Read
	* Update
	* Create
	* Delete
	* SQL Functions
10. In-Class Activity

---

### What is SQL?
**S**tructured
**Q**uery
**L**anguage
Many database types use SQL
	* SQLite
	* PostgreSQL
	* MySQL
	* Oralce
	* Microsoft SQL Server

NoSQL Databases:
	* Cassandra
	* CouchDB
	* HBase
	* MongoDB
	* Redis

### CRUD
| CRUD      | HTML Verbs    | SQL Commands  |
| --------- |:-------------:| -------------:|
| Create    | post          | insert        |
| Read      | get           | select        |
| Update    | patch/put     | update        |
| Delete    | delete        | delete        |

Notice the HTML Verbs from our time in the **routes.rb** file?

### Create a New App
Let's start a new project...

```sh
$ rails new db_explore
$ cd db_explore
```

...and create a Resource called User...

```sh
$ rails g scaffold User name:string location:string age:integer
$ rails db:migrate
```

Then we need some data to explore: create five Users!


### rails dbconsole
Let's use SQL querying to take a look inside our database...
Open the dbconsole:

```sh
db_explore$ rails dbconsole

SQLite version 3.8.10.2 2015-05-20 18:17:19

Enter ".help" for usage hints.

sqlite> 
```


### SQL Queries
Inside the DB Console, let's practice SQL querying.

First up, we can check to see what Tables we're working with (Perhaps you've simply forgotten what databases you even have on this project)...

```sh
sqlite> .tables

schema_migrations users

sqlite>
```

Next, we'll see how to Read - *INSERT* in SQL. But you need to say more than just "Select" to run a query. 

In SQL, the asterisk (*) means "all". Let's pull all the data from the User table!

```sh
sqlite> SELECT * FROM users;

1|Jude|Atlanta|19|2014-08-18 12:55:45.899020|2014-08-18 12:56:37.077430

2|Ellen|San Diego|65|2014-08-18 12:56:00.461203|2014-08-18 12:56:00.461203

3|Norah|Venice|32|2014-08-18 12:56:18.187149|2014-08-18 12:56:18.187149

4|Jason|Indianapolis|22|2014-08-18 12:56:31.751468|2014-08-18 12:56:31.751468

5|Leila|Edmonton|29|2014-08-18 12:56:54.591946|2014-08-18 12:56:54.591946

sqlite>
```

Note that the SQL keywords (Select, From), don't need to be in all-caps, but it helps differentiate those words from the non-SQL words.

#### SQL Modes

Before we move on...What if we wanted to change the look of the query output?
 
Let's type in .help and find .mode to see our choices:
```sh
.mode MODE ?TABLE?     Set output mode where MODE is one of:
												ascii    Columns/rows delimited by 0x1F and 0x1E
												csv      Comma-separated values
												column   Left-aligned columns.  (See .width)
												html     HTML <table> code
												insert   SQL insert statements for TABLE
												line     One value per line
												list     Values delimited by .separator strings
												tabs     Tab-separated values
												tcl      TCL list elements
```

Let's try out the "column" mode. You can also turn on headers:

```sh
sqlite> .mode column
splite> .headers on
sqlite> SELECT * FROM users;
id          name        location    age         created_at                  updated_at                
----------  ----------  ----------  ----------  --------------------------  --------------------------
1           Jude        Atlanta     19          2014-08-18 12:55:45.899020  2014-08-18 12:56:37.077430

2           Ellen       San Diego   65          2014-08-18 12:56:00.461203  2014-08-18 12:56:00.461203

3           Norah       Venice      32          2014-08-18 12:56:18.187149  2014-08-18 12:56:18.187149

4           Jason       Indianapol  22          2014-08-18 12:56:31.751468  2014-08-18 12:56:31.751468

5           Leila       Edmonton    29          2014-08-18 12:56:54.591946  2014-08-18 12:56:54.591946

sqlite>
```

Check out some of the other .mode types. If you liked the original, just set back to ".mode list".

Now, back into SELECT queries...

Maybe we don't want to see everything. Maybe we just want to see certain attributes (columns).

```sh
sqlite> SELECT name, age FROM users;

Jude|19

Ellen|65

Norah|32

Jason|22

Leila|29

sqlite>
```

What if we had hundreds of rows in our table? Do we want to see all of those at once? Nah – so let's “limit” what's returned...

```sh
sqlite> SELECT * FROM users LIMIT 2;

1|Jude|Atlanta|19|2014-08-18 12:55:45.899020|2014-08-18 12:56:37.077430

2|Ellen|San Diego|65|2014-08-18 12:56:00.461203|2014-08-18 12:56:00.461203

sqlite>
```

What if we're looking for a certain person?

```sh
sqlite> SELECT * FROM users WHERE name = 'Jason';

4|Jason|Indianapolis|22|2014-08-18 12:56:31.751468|2014-08-18 12:56:31.751468

sqlite>

sqlite> SELECT * FROM users WHERE name = 'Jason';

4|Jason|Indianapolis|22|2014-08-18 12:56:31.751468|2014-08-18 12:56:31.751468

sqlite>
```
In this case, we use the 'where' clause to get specific.
 
You can also get a little more complex:

```sh
sqlite> SELECT * FROM users WHERE name = 'Leila' AND age = 29;

5|Leila|Edmonton|29|2014-08-18 12:56:54.591946|2014-08-18 12:56:54.591946

sqlite>
```
Using 'or' is also an option.

You can use greater and less than, too (with integers or floats, of course):

sqlite> SELECT * FROM users WHERE age > 30;

2|Ellen|San Diego|65|2014-08-18 12:56:00.461203|2014-08-18 12:56:00.461203

3|Norah|Venice|32|2014-08-18 12:56:18.187149|2014-08-18 12:56:18.187149

sqlite>
```

#### SELECT Functions
Functions are built-in utilities that assist us in performing some calculations. Here are just a few of the useful functions we can use:

	* AVG (average)
	* SUM
	* COUNT
	* MIN (minimum)
	* MAX (maximum)
	* UPPER
	* DISTINCT

[List of Numeric Functions](http://www.tutorialspoint.com/sql/sql-numeric-functions.htm)
[List of String Functions](http://www.tutorialspoint.com/sql/sql-string-functions.htm)

#### SQL Updating
Just looking at the data is all well and good, but what about changing the data?

```sh
sqlite> SELECT * FROM users WHERE id = 1;
1|Jude|Atlanta|19|2014-11-10 23:31:26.585649|2014-11-10 23:31:26.585649
sqlite> UPDATE users SET age = 25 WHERE id = 1;
sqlite>
```

Nothing seems to happen when we "update", but check  again!

```sh
sqlite> SELECT * FROM users WHERE id = 1;
1|Jude|Atlanta|25|2014-11-10 23:31:26.585649|2014-11-10 23:31:26.585649
sqlite>
```

#### Creating with SQL
We can create new entries with SQL:

```sh
sqlite> INSERT INTO users (name, location, age, updated_at, created_at) VALUES 
('Sam', 'Pensacola', 33, CURRENT_TIMESTAMP, CURRENT_TIMESTAMP);
sqlite> SELECT * FROM users WHERE name = 'Sam';
7|Sam|Pensacola|33|2015-08-10 01:39:17|2015-08-10 01:39:17
sqlite>
```

Unlike when we're running the server and creating new instances of resources (which is using Active Record), creating with SQL gives us an ID, but will not give us "created_at" and "updated_at" automatically - it's up to us to fill those in, because they Rails has deemed them *required* fields. 

"CURRENT_TIMESTAMP" will populate the created_at and updated_at fields with the current date-time.

#### Deleting in SQL
We can delete an entry via SQL...

```sh
sqlite> DELETE FROM users WHERE name = 'Jason';

sqlite> SELECT * FROM users;

1|Jude|Atlanta|25|2014-08-18 12:55:45.899020|2014-08-18 12:56:37.077430

2|Ellen|San Diego|65|2014-08-18 12:56:00.461203|2014-08-18 12:56:00.461203

3|Norah|Venice|32|2014-08-18 12:56:18.187149|2014-08-18 12:56:18.187149

5|Leila|Edmonton|29|2014-08-18 12:56:54.591946|2014-08-18 12:56:54.591946

6|Sam|Pensacola|33|2015-08-10 01:39:17|2015-08-10 01:39:17

sqlite>
```

Note that not only is Jason gone, but ID #4 is gone & the table hasn't been re-ordered. #4 is forever gone from this table!

Then, to leave the DBConsole:
```sh
sqlite> .exit
$
```


### Active Record Intro
Active Record makes SQL easy. Active Record is Ruby methods storing SQL Queries. And you've already seen it at work in your Controllers:

```ruby
def index
  @users = User.all
end

def new
  @user = User.new
end
```


### Rails Console
ActiveRecord has its own console:

```sh
Running via Spring preloader in process 20820
Loading development environment (Rails 5.0.2)
2.2.4 :001 > 
$ rails console
Running via Spring preloader in process 20820
Loading development environment (Rails 5.0.2)
2.2.4 :001 > 
```
And just like "rails server" can be shortened to "rails s",
"rails console" can be shortened to "rails c"

That's our Ruby version, followed by a line number for this session of rails console. Rails console is basically irb, but hooked up to our database.


### Active Record
We'll look through Active Record's CRUD queries in the same order we did for SQL. So we start with Read.

We make Active Record's call on the resource name (in this case: User).

```sh
:001 > User.all
  User Load (3.7ms)  SELECT "users".* FROM "users"
 => #<ActiveRecord::Relation [#<User id: 1, name: "Jude", location: "Atlanta", age: 25, 
created_at: "2014-11-10 23:31:26", updated_at: "2014-11-10 23:31:26">, #<User id: 2, 
name: "Ellen", location: "San Diego", age: 65, created_at: "2014-11-10 23:31:36", 
updated_at: "2014-11-10 23:31:36">, #<User id: 3, name: "Norah", location: "Venice", 
age: 32, created_at: "2014-11-10 23:31:46", updated_at: "2014-11-10 23:31:46">, 
#<User id: 4, name: "Jason", location: "Indianapolis", age: 22, created_at: 
"2014-11-10 23:31:58", updated_at: "2014-11-10 23:31:58">, #<User id: 5, name: "Leila", 
location: "Edmonton", age: 29, created_at: "2014-11-10 23:32:10", updated_at: 
"2014-11-10 23:32:10">, #<User id: 6, name: "Sam", location: "Pensacola", age: 33, 
created_at: "2015-08-10 01:39:17", updated_at: "2015-08-10 01:39:17">]>
:002 >
:001 > User.all
  User Load (3.7ms)  SELECT "users".* FROM "users"
 => #<ActiveRecord::Relation [#<User id: 1, name: "Jude", location: "Atlanta", age: 25, 
created_at: "2014-11-10 23:31:26", updated_at: "2014-11-10 23:31:26">, #<User id: 2, 
name: "Ellen", location: "San Diego", age: 65, created_at: "2014-11-10 23:31:36", 
updated_at: "2014-11-10 23:31:36">, #<User id: 3, name: "Norah", location: "Venice", 
age: 32, created_at: "2014-11-10 23:31:46", updated_at: "2014-11-10 23:31:46">, 
#<User id: 4, name: "Jason", location: "Indianapolis", age: 22, created_at: 
"2014-11-10 23:31:58", updated_at: "2014-11-10 23:31:58">, #<User id: 5, name: "Leila", 
location: "Edmonton", age: 29, created_at: "2014-11-10 23:32:10", updated_at: 
"2014-11-10 23:32:10">, #<User id: 6, name: "Sam", location: "Pensacola", age: 33, 
created_at: "2015-08-10 01:39:17", updated_at: "2015-08-10 01:39:17">]>
:002 >
```

The output format may be uglier than SQL's, but the command is so much easier!

Anyway, there's gems we can use to change the look of Rails Console.

Also, notice how it shows you the SQL query.

We can look at specific entries...

```sh
  User Load (0.2ms)  SELECT  "users".* FROM "users"   ORDER BY "users"."id" ASC LIMIT 1
 => #<User id: 1, name: "Jude", location: "Atlanta", age: 25, 
created_at: "2014-11-10 23:31:26", updated_at: "2014-11-10 23:31:26">
:003 > User.last
  User Load (0.2ms)  SELECT  "users".* FROM "users"   ORDER BY "users"."id" DESC LIMIT 1
 => #<User id: 6, name: "Sam", location: "Pensacola", age: 33, created_at: nil, 
updated_at: nil>
:004 >
:002 > User.first
  User Load (0.2ms)  SELECT  "users".* FROM "users"   ORDER BY "users"."id" ASC LIMIT 1
 => #<User id: 1, name: "Jude", location: "Atlanta", age: 25, 
created_at: "2014-11-10 23:31:26", updated_at: "2014-11-10 23:31:26">
:003 > User.last
  User Load (0.2ms)  SELECT  "users".* FROM "users"   ORDER BY "users"."id" DESC LIMIT 1
 => #<User id: 6, name: "Sam", location: "Pensacola", age: 33, created_at: nil, 
updated_at: nil>
:004 >
```

We can give ActiveRecords specifics...
```sh
:004 > User.find(3)
  User Load (0.3ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 3]]
 => #<User id: 3, name: "Norah", location: "Venice", age: 32, created_at: "2014-11-10 23:31:46", 
updated_at: "2014-11-10 23:31:46">
:005 > User.find_by(location: "San Diego")
  User Load (0.3ms)  SELECT  "users".* FROM "users"  WHERE "users"."location" = 'San Diego' LIMIT 1 
 => #<User id: 2, name: "Ellen", location: "San Diego", age: 65, created_at: "2014-11-10 23:31:36", 
updated_at: "2014-11-10 23:31:36">
:006 > User.where(name: "Jason")
  User Load (0.2ms)  SELECT "users".* FROM "users"  WHERE "users"."name" = 'Jason'
 => #<ActiveRecord::Relation [#<User id: 4, name: "Jason", location: "Indianapolis", age: 22, 
created_at: "2014-11-10 23:31:58", updated_at: "2014-11-10 23:31:58">]>
:007 >
```

Notice the difference between ".find_by" & ".where": one is a single entry, the other a collection.

Next, let's Update...

Updating in Active Record is a bit more complicated than in SQL, but it's exactly how you'd update an entry in your Rails project.

```sh
:007> x = User.find(3)
  User Load (0.1ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 3]]
 => #<User id: 3, name: "Norah", location: "Venice", age: 32, created_at: "2014-11-10 23:31:46", 
updated_at: "2014-11-10 23:31:46">
:008 > x.location = "Chicago"
 => "Chicago"
:009 > User.find(3)
  User Load (0.1ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 3]]
 => #<User id: 3, name: "Norah", location: "Venice", age: 32, created_at: "2014-11-10 23:31:46", 
updated_at: "2014-11-10 23:31:46">
:010 > x.save
   (0.1ms)  begin transaction
  SQL (0.6ms)  UPDATE "users" SET "location" = ?, "updated_at" = ? WHERE "users"."id" = 3  
[["location", "Chicago"], ["updated_at", "2014-11-11 11:52:22.885042"]]
   (0.9ms)  commit transaction
 => true
:011 > User.find(3)
  User Load (0.1ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 3]]
 => #<User id: 3, name: "Norah", location: "Chicago", age: 32, created_at: "2014-11-10 23:31:46", 
updated_at: "2014-11-11 11:52:22">
```

The change isn't saved until we call 'x.save'.

But you know how we like to keep things DRY... so what if we could make a change in just one line?

```sh
:012> x = User.find(3)
  User Load (0.1ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 3]]
 => #<User id: 3, name: "Norah", location: "Chicago", age: 32, created_at: "2014-11-10 23:31:46", 
updated_at: "2014-11-10 23:31:46">
:013 > x.update(age: 33)
   (0.1ms)  begin transaction
  SQL (0.6ms)  UPDATE "users" SET "location" = ?, "updated_at" = ? WHERE "users"."id" = 3  
[["age", 33], ["updated_at", "2014-11-11 11:52:22.885042"]]
   (0.9ms)  commit transaction
 => true
:014 > User.find(3)
  User Load (0.1ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 3]]
 => #<User id: 3, name: "Norah", location: "Chicago", age: 33, created_at: "2014-11-10 23:31:46", 
updated_at: "2014-11-11 11:52:22">
```

We didn't have to call 'x.save' - '.update' does the changing and the updating all in one!

Next we'll Create...

Just like Updating, there's two ways we can go about this in Active Record. In one way, first we create a new (but empty) instance of a User:
```sh
:015 > newbie = User.new
 => #<User id: nil, name: nil, location: nil, age: nil, created_at: nil, updated_at: nil>
```

Next, give your newbie a name, location and age. Active Record will take care of the rest:
```sh
:016 > newbie.name = "Josephine"
 => "Josephine"
:017 > newbie.location = "Santiago"
 => "Santiago"
:018 > newbie.age = 41
 => 41
:019 > newbie.save
   (0.1ms)  begin transaction
  SQL (0.3ms)  INSERT INTO "users" ("age", "created_at", "id", "location", "name", "updated_at") 
VALUES (?, ?, ?, ?, ?, ?)  [["age", 41], ["created_at", "2014-11-11 12:51:03.809027"],  
["location", "Santiago"], ["name", "Josephine"], ["updated_at", "2014-11-11 12:51:03.809027"]]
   (1.1ms)  commit transaction
 => true
:020 > User.where(name: "Josephine")
  User Load (0.3ms)  SELECT "users".* FROM "users"  WHERE "users"."name" = 'Josephine'
 => #<ActiveRecord::Relation [#<User id: 7, name: "Josephine", location: "Santiago", age: 41, 
created_at: "2014-11-11 12:51:03", updated_at: "2014-11-11 12:51:03">]>
```

But that's so many lines of code! Can't we get it done in just one line?
Yes we can! With .create()!

```sh
:021 > Newbie.create(name: "Daphne", location: "Spokane", age: 31)
   (0.1ms)  begin transaction
  SQL (0.4ms)  INSERT INTO "users" ("name", "location", "age", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?)  [["name", "Daphne"], ["location", "Spokane"], ["age", 31], ["created_at", "2016-04-18 16:20:16.797104"], ["updated_at", "2016-04-18 16:20:16.797104"]]
   (1.2ms)  commit transaction
 => #<User id: 1, name: "Daphne", location: "Spokane", age: 31, created_at: "2016-04-18 16:20:16", updated_at: "2016-04-18 16:20:16"> 
:022 > User.last
  User Load (0.3ms)  SELECT  "users".* FROM "users"  ORDER BY "users"."id" DESC LIMIT 1
 => #<User id: 1, name: "Daphne", location: "Spokane", age: 31, created_at: "2016-04-18 16:20:16", updated_at: "2016-04-18 16:20:16">
```
We don't even need to do .save

Finally, Destroying in Active Record...

```sh
:023 > User.find(7).destroy
  User Load (0.1ms)  SELECT  "users".* FROM "users"  WHERE "users"."id" = ? LIMIT 1  [["id", 7]]
   (0.1ms)  begin transaction
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 7]]
   (0.8ms)  commit transaction
 => #<User id: 7, name: "Josephine", location: "Santiago", age: 41, created_at: "2014-11-11 13:02:34", 
updated_at: "2014-11-11 13:02:34"> 
:024 >
```

*.destroy* works only on single entries. For multiple entries you need *.destroy_all*
Here's what it would look like, but maybe don't actually run *.destroy_all*:

```sh
:024 > User.destroy_all
  User Load (0.3ms)  SELECT "users".* FROM "users"
   (0.1ms)  begin transaction
  SQL (0.8ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 2]]
   (1.1ms)  commit transaction
   (0.1ms)  begin transaction
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 3]]
   (0.9ms)  commit transaction
   (0.1ms)  begin transaction
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 4]]
   (0.9ms)  commit transaction
   (0.1ms)  begin transaction
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 5]]
   (0.9ms)  commit transaction
   (0.1ms)  begin transaction
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 6]]
   (0.9ms)  commit transaction
   (0.0ms)  begin transaction
  SQL (0.1ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 7]]
   (5.6ms)  commit transaction
   (0.1ms)  begin transaction
  SQL (0.2ms)  DELETE FROM "users" WHERE "users"."id" = ?  [["id", 8]]
   (0.8ms)  commit transaction
 => [#<User id: 2, name: "Jude", location: "Atlanta", age: 25, created_at: "2016-04-18 16:36:13", updated_at: "2016-04-18 16:36:13">, #<User id: 3, name: "Ellen", location: "San Diego", age: 65, created_at: "2016-04-18 16:36:29", updated_at: "2016-04-18 16:36:29">, #<User id: 4, name: "Norah", location: "Chicago", age: 32, created_at: "2016-04-18 16:36:43", updated_at: "2016-04-18 16:39:02">, #<User id: 5, name: "Jason", location: "Indianapolis", age: 32, created_at: "2016-04-18 16:36:57", updated_at: "2016-04-18 16:36:57">, #<User id: 6, name: "Leila", location: "Edmonton", age: 29, created_at: "2016-04-18 16:37:10", updated_at: "2016-04-18 16:37:10">, #<User id: 7, name: "Sam", location: "Pensacola", age: 33, created_at: "2016-04-18 16:37:24", updated_at: "2016-04-18 16:37:24">, #<User id: 8, name: "Daphne", location: "Spokane", age: 31, created_at: "2016-04-18 16:39:18", updated_at: "2016-04-18 16:39:18">]
```

#### SQL Functions in Active Record
Remember those SELECT functions we saw with SQL? ActiveRecord has those covered, too!

```sh
:024 > User.average(:age).to_i
    (0.2ms)  SELECT AVG("users"."age") FROM "users"
=> 34
:025 > User.sum(:age)
    (0.2ms)  SELECT SUM("users"."age") FROM "users"
=> 206
:026 > User.maximum(:age)
    (35.3ms)  SELECT MAX("users"."age") FROM "users"
=> 65
```

Find more [ActiveRecord Calculations](http://api.rubyonrails.org/classes/ActiveRecord/Calculations.html)!
Distinct is here, too:

```sh
:027 > Users.select(:city).distinct
    User Load (1.0ms)  SELECT DISTINCT "users"."location" FROM "users"
 => #<ActiveRecord::Relation [#<User id: nil, location: "Atlanta">, 
#<User id: nil, location: "San Diego">, #<User id: nil, location: "Venice">, 
#<User id: nil, location: "Indianapolis">, #<User id: nil, location: "Edmonton">, 
#<User id: nil, location: "Pensacola">]>
```

Even leaving Rails Console is easier than leaving DB Console:

```sh
:028 > exit
db_explore$ 
```
Okay... it's just missing the dot before "exit". Still technically easier!

And now you can get back to generating, scaffolding, and running the server.


### In-Class Activity
*Instructor: lead students this this activity, but ask for input along the way*

Create New Project: furniture store
```sh
$ rails new furniture-store
```
 
Create a new resource: Product (name, price, category)
We can generate just a new model/table with this command:
```sh
$ rails g model Product name:string price:decimal category:string
```

Use Rails Console to populate table.
For example:
```sh
:001 > Product.create("Sofa", 499.99, "Living Room")
```
 
Create new Controller: Inventory
w/ these pages: all_products, one_product, by_category
```sh
$ rails g controller Inventory all_products one_product by_category
```
 
Use Active Record calls to populate each page
```ruby
# inventory_controller.rb
class InventoryController < ApplicationController
  def all_products
  	@products = Product.all
  end

  def one_product
  	@product = Product.find(params[:id])
  end

  def by_category
  	@category = params[:category]
  	@products = Product.where(category: @category)
  end
end
```

Views:
```html
<!-- all_products.html.erb -->
<h1>Our Entire Inventory!</h1>

<table>
	<tr>
		<th>Name</th>
		<th>Price</th>
		<th>Category</th>
	</tr>

	<% @products.each do |product| %>
		<tr>
			<td><%= link_to product.name, product_path(id: product.id) %></td>
			<td><%= number_to_currency(product.price) %></td>
			<td><%= link_to product.category, categorized_path(category: product.category) %></td>
		</tr>
	<% end %>
</table>
```

```html
<!-- by_category.html.erb -->
<h1>Our Entire <%= @category %> Stock:</h1>

<table>
	<tr>
		<th>Name</th>
		<th>Price</th>
		<th>Category</th>
	</tr>

	<% @products.each do |product| %>
		<tr>
			<td><%= product.name %></td>
			<td><%= number_to_currency(product.price) %></td>
			<td><%= product.category %></td>
		</tr>
	<% end %>
</table>
```

```html
<!-- one_product.html.erb -->
<h1><%= @product.name %></h1>

<p>
	<strong>Price: </strong>
	<%= number_to_currency(@product.price) %>
</p>

<p>
	<strong>Category: </strong>
	<%= link_to @product.category, categorized_path(category: @product.category) %>
</p>
```

Routes:
```ruby
# routes.rb
Rails.application.routes.draw do

  get 'products' => 'inventory#all_products'

  get 'product' => 'inventory#one_product'

  get 'categorized' => 'inventory#by_category'

end
```