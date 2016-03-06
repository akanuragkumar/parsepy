ParsePy
ParsePy is a Python client for the Parse REST API. It provides Python object mapping for Parse objects with methods to save, update, and delete objects, as well as an interface for querying stored objects.

Basic Usage

Let's get everything set up first. You'll need to give ParsePy your Application Id and Master Key (available from your Parse dashboard) in order to get access to your data.

>>> import ParsePy
>>> ParsePy.APPLICATION_ID = "your application id"
>>> ParsePy.MASTER_KEY = "your master key here"
To create a new object of the Parse class GameScore:

>>> gameScore = ParsePy.ParseObject("GameScore")
>>> gameScore.score = 1337
>>> gameScore.playerName = "Sean Plott"
>>> gameScore.cheatMode = False
As you can see, we add new properties simply by assigning values to our ParseObject's attributes. Supported data types are any type that can be serialized by JSON and Python's datetime.datetime object. (Binary data and references to other ParseObject's are also supported, as we'll see in a minute.)

To save our new object, just call the save() method:

>>> gameScore.save()
If we want to make an update, just call save() again after modifying an attribute to send the changes to the server:

>>> gameScore.score = 2061
>>> gameScore.save()
Now that we've done all that work creating our first Parse object, let's delete it:

>>> gameScore.delete()
That's it! You're ready to start saving data on Parse.

Object Metadata

The methods objectId(), createdAt(), and updatedAt() return metadata about a ParseObject that cannot be modified through the API:

>>> gameScore.objectId()
'xxwXx9eOec'
>>> gameScore.createdAt()
datetime.datetime(2011, 9, 16, 21, 51, 36, 784000)
>>> gameScore.updatedAt()
datetime.datetime(2011, 9, 118, 14, 18, 23, 152000)
Additional Datatypes

If we want to store data in a ParseObject, we should wrap it in a ParseBinaryDataWrapper. The ParseBinaryDataWrapper behaves just like a string, and inherits all of str's methods.

>>> gameScore.victoryImage = ParsePy.ParseBinaryDataWrapper('\x03\xf3\r\n\xc7\x81\x7fNc ... ')
We can store a reference to another ParseObject by assigning it to an attribute:

>>> collectedItem = ParsePy.ParseObject("CollectedItem")
>>> collectedItem.type = "Sword"
>>> collectedItem.isAwesome = True
>>> collectedItem.save() # we have to save it before it can be referenced

>>> gameScore.item = collectedItem
Querying

To retrieve an object with a Parse class of GameScore and an objectId of xxwXx9eOec, run:

>>> gameScore = ParsePy.ParseQuery("GameScore").get("xxwXx9eOec")
We can also run more complex queries to retrieve a range of objects. For example, if we want to get a list of GameScore objects with scores between 1000 and 2000 ordered by playerName, we would call:

>>> query = ParsePy.ParseQuery("GameScore")
>>> query = query.gte("score", 1000).lt("score", 2000).order("playerName")
>>> GameScores = query.fetch()
Notice how queries are built by chaining filter functions. The available filter functions are:

Less Than
lt(parameter_name, value)
Less Than Or Equal To
lte(parameter_name, value)
Greater Than
gt(parameter_name, value)
Greater Than Or Equal To
gte(parameter_name, value)
Not Equal To
ne(parameter_name, value)
Limit
limit(count)
Skip
skip(count)
We can also order the results using:

Order
order(parameter_name, decending=False)
