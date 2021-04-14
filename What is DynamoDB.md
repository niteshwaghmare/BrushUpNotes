# What is DynamoDB?

[DynamoDB](https://aws.amazon.com/dynamodb/) is a hosted NoSQL database offered by Amazon Web Services (AWS). It offers:

- *reliable performance* even as it scales;
- a *managed experience*, so you won't be SSH-ing into servers to upgrade the crypto libraries;
- a *small, simple API* allowing for simple key-value access as well as more advanced query patterns.

DynamoDB is a particularly good fit for the following use cases:

**Applications with large amounts of data and strict latency requirements**. As your amount of data scales, JOINs and advanced SQL operations can slow down your queries. With DynamoDB, your queries have predictable latency up to any size, including [over 100 TBs](https://medium.com/building-timehop/one-year-of-dynamodb-at-timehop-f761d9fe5fa1)!

**Serverless applications using AWS Lambda**. [AWS Lambda](https://aws.amazon.com/lambda/) provides auto-scaling, stateless, ephemeral compute in response to event triggers. DynamoDB is accessible via an HTTP API and performs authentication & authorization via IAM roles, making it a perfect fit for building Serverless applications.

**Data sets with simple, known access patterns**. If you're generating recommendations and serving them to users, DynamoDB's simple key-value access patterns make it a fast, reliable choice. 

### Ready to learn more?

Start with the [key concepts](https://www.dynamodbguide.com/key-concepts) to learn about tables, items, and other basic elements of DynamoDB. If you want the computer science background on DynamoDB, check out the section on the [Dynamo Paper](https://www.dynamodbguide.com/the-dynamo-paper). 

If you want to get your hands dirty, [set up your environment](https://www.dynamodbguide.com/environment-setup) then start with the section on [working with single items](https://www.dynamodbguide.com/anatomy-of-an-item). Then you can move on to [working with multiple items](https://www.dynamodbguide.com/working-with-multiple-items) using Queries and Scans. 

Want the advanced stuff? Power up your tables with [secondary indexes](https://www.dynamodbguide.com/secondary-indexes) and [DynamoDB Streams](https://www.dynamodbguide.com/dynamodb-streams).

Still want more? Head to [Additional Reading](https://www.dynamodbguide.com/additional-reading) to find the best community resources on DynamoDB.

# Key Concepts

In this section, we'll cover the key concepts you need to know about DynamoDB. At the end of this section, you will understand:

- **tables**, **items**, and **attributes**; 
- **primary keys**;
- **secondary indexes**; 
- **read and write capacity**.

### Tables, Items, and Attributes

Tables, items, and attributes are the core building blocks of DynamoDB.

A *table* is a grouping of data records. For example, you might have a Users table to store data about your users, and an Orders table to store data about your users' orders. This concept is similar to a table in a relational database or a collection in MongoDB.

An *item* is a single data record in a table. Each item in a table is uniquely identified by the stated [primary key](https://www.dynamodbguide.com/key-concepts#primary-key) of the table. In your Users table, an item would be a particular User. An item is similar to a row in a relational database or a document in MongoDB.

*Attributes* are pieces of data attached to a single item. This could be a simple Age attribute that stores the age of a user. An attribute is comparable to a column in a relational database or a field in MongoDB. DynamoDB does not require attributes on items except for attributes that make up your [primary key](https://www.dynamodbguide.com/key-concepts#primary-key).

### Primary Key

Each item in a table is uniquely identified by a primary key. The primary key definition must be defined at the creation of the table, and the primary key must be provided when inserting a new item.

There are two types of primary key: a **simple primary key** made up of just a partition key, and a **composite primary key** made up of a partition key and a sort key.

Using a **simple primary key** is similar to standard key-value stores like Memcached or accessing rows in a SQL table by a primary key. One example would be a Users table with a Username primary key.

The **composite primary key** is more complex. With a composite primary key, you specify both a partition key and a sort key. The sort key is used to (wait for it) *sort* items with the same partition. One example could be an Orders tables for recording customer orders on an e-commerce site. The partition key would be the CustomerId, and the sort key would be the OrderId.

Remember: *each item in a table is uniquely identified by a primary key*, even with the composite key. When using a table with a composite primary key, you may have multiple items with the same partition key but different sort keys. You can only have one item with a particular combination of partition key and sort key.

The composite primary key enables [sophisticated query patterns](https://www.dynamodbguide.com/working-with-multiple-items), including grabbing all items with the given partition key or using the sort key to narrow the relevant items for a particular query.

For more on interacting with items, start with the lesson on the [anatomy of an item](https://www.dynamodbguide.com/anatomy-of-an-item).

### Secondary Indexes

The primary key uniquely identifies an item in a table, and you may make queries against the table using the primary key. However, sometimes you have additional access patterns that would be inefficient with your primary key. DynamoDB has the notion of **secondary indexes** to enable these additional access patterns.

The first kind of secondary index is a **local secondary index**. A local secondary index uses the same partition key as the underlying table but a different sort key. To take our Order table example from the previous section, imagine you wanted to quickly access a customer's orders in descending order of the amount they spent on the order. You could add a local secondary index with a partition key of CustomerId and a sort key of Amount, allowing for efficient queries on a customer's orders by amount.

The second kind of secondary index is a **global secondary index**. A global secondary index can define an entirely different primary key for a table. This could mean setting an index with just a partition key for a table with a composite primary key. It could also mean using completely different attributes to populate a partition key and sort key. With the Order example above, we could have a global secondary index with a partition key of OrderId so we could retrieve a particular order without knowing the CustomerId that placed the order.

Secondary indexes are a complex topic but are extremely useful in getting the most out of DynamoDB. Check out the section on [secondary indexes](https://www.dynamodbguide.com/secondary-indexes) for a deeper dive.

# Read and Write Capacity

When you use a database like MySQL, Postgres, or MongoDB, you provision a particular server to run your database. You'll need to choose your instance size -- how many CPUs do you need, how much RAM, how many GBs of storage, etc.

Not so with DynamoDB. Instead, you provision *read and write capacity units*. These units allow a given number of operations per second. This is a fundamentally different pricing paradigm than the instance-based world -- pricing can more closely reflect actual usage.

DynamoDB also has [autoscaling](https://www.dynamodbguide.com/autoscaling) of your read and write capacity units. This makes it much easier to scale your application up during peak times while saving money by scaling down when your users are asleep.

# The Dynamo Paper

In 2004, [Amazon.com](http://amazon.com/) was growing rapidly and was starting to hit the upper scaling limits of its Oracle database. It started to consider building its own database in-house (*note to readers: this is almost always a bad idea*). Out of this experiment, the engineers created the Amazon Dynamo database which backed major internal infrastructure including the shopping cart on the [Amazon.com](http://amazon.com/) website. 

A group of engineers behind the Amazon Dynamo database published the [Dynamo Paper](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) in 2007. It described the learnings from building an in-house, highly available key-value store designed to meet the demanding requirements of the [Amazon.com](http://amazon.com/) website.

The paper was highly influential and inspired a number of NoSQL databases, including Apache Cassandra (originally developed at Facebook) and AWS offerings SimpleDB and DynamoDB. In 2012, Amazon Web Services launched DynamoDB, which was a managed database service modeled after the principles behind Dynamo.

> Want to know more about how DynamoDB scales? Check out this post on [SQL, NoSQL, and Scale: How DynamoDB scales where relational databases don't](https://www.alexdebrie.com/posts/dynamodb-no-bad-queries/).

## Key aspects of Dynamo

#### No Relational model

The relational data model is a useful way to model many types of data. Often, relational data is [normalized](https://en.wikipedia.org/wiki/Database_normalization) to improve the integrity of the data. Rather than duplicating a particular piece of data in multiple rows, you can store it in one place and refer to it using a JOIN operation from one table to another. Now you can update that single place, and all items that refer to that data will gain the benefits of the update as well.

Yet one of the most interesting findings of the [Amazon.com](http://amazon.com/) engineers while gathering their database requirements was how their engineers were using their relational databases:

> About 70 percent of operations were of the key-value kind, where only a primary key was used and a single row would be returned. About 20 percent would return a set of rows, but still operate on only a single table.
>
> \- Werner Vogels, [*A Decade of Dynamo*](http://www.allthingsdistributed.com/2017/10/a-decade-of-dynamo.html)

This is a huge deal -- *90% of operations weren't using the JOIN functionality that is core to a relational database!*

The JOIN operation is expensive. At a large enough scale, engineers often denormalize their data to avoid making expensive joins and slowing down response times. This decrease in response time comes with a trade-off of increased application complexity -- now you need to manage more of your data integrity issues in your code rather than your database.

[Amazon.com](http://amazon.com/) engineers were already making that trade-off of denormalization to improve response times. The realization that the relational model wasn't needed by Amazon engineers allowed the Dynamo designers to re-evaluate other aspects of a relational database.

#### Availability > Consistency

Most relational databases use a *strongly consistent* model for their data. Briefly, this means all clients of the server will see the same data if querying at the same time. 

Let's use Twitter as an example. Imagine that Bob in Virginia tweets a cat picture at 2:30 PM. There are two users that view Bob's profile after he tweets his picture: his neighbor, Cheryl, and his uncle, Jeffrey, who lives in Singapore. If Twitter were using a strongly-consistent model, both Cheryl and Jeffrey should see Bob's most recent tweet as soon as it's committed to the database from Bob's action.

This might not be ideal, for a few reasons. First, think of the geography involved in this scenario. Twitter could choose to have a single database instance to enable this strong consistency. This database instance may be located in Virginia, close to Bob and Cheryl. This results in fast responses to Bob and Cheryl, but very slow responses to Jeffrey as each request must cross an ocean from Singapore to Virginia to request the data, then return from Virginia to Singapore to return it to Jeffrey. *This results in slower read times to some users*.

Instead of maintaining a single database instance, perhaps Twitter wants to have two instances that are exact replicas -- one in Virginia and one in Singapore. If we still want to maintain strong consistency, this means a user must get the same answer if she queries the Virginia instance or the Singapore instance at the same time. This could be implemented by a more complex system on database writes -- before Bob's tweet is committed to the database, it has to be submitted to both the Virginia instance and the Singapore instance. Now Bob's request needs to make the hop across the ocean and back. *This results in slower write times to some users*.

In the Dynamo paper, Amazon noted that strong consistency isn't important in all scenarios. In our example, it would be fine if Jeffrey and Cheryl saw slightly different versions of my profile even if they queried at the same time. Sometimes you can settle for *eventual consistency*, meaning different users will eventually see the same view of the data. Jeffrey will eventually see Bob's tweet in Singapore, but it may be at 2:32 PM rather than 2:30.

Strong consistency is important for certain use cases - think bank account balances - but less important for others, such as our Twitter example or the Amazon shopping cart, which was the impetus for Dynamo. For these use cases, speed and availability are more important than a consistent view of the world. By weakening the consistency model of a relational database, the Dynamo engineers were able to provide a database that better fit the needs of [Amazon.com](http://amazon.com/).

*Note: This section is a massive simplification of consistency, availability, and other concepts around databases and distributed systems. You should really look at this as a very simple primer rather than a definitive text.*

#### Infinitely Scalable

The final key aspect of Dynamo is that it is infinitely scalable without any negative performance impacts. This aspect is a result of the relaxing of relational and consistency constraints from prior databases.

When scaling out a system, you can either vertically scale (use a larger server instance with more CPUs or RAM) or you can horizontally scale by splitting your data across multiple machines, each of which has a subset of your full dataset. Vertical scaling gets expensive and eventually hits limits based on available technology. Horizontal scaling is cheaper but more difficult to achieve.

To think about horizontal scaling, imagine you have a dataset of Users that you want to distribute across three machines. You could choose to split them across machines based on the last name of the Users -- A through H go on machine 1, I through Q go on machine 2, and R through Z go on machine 3. 

This is nice if you're getting a single User -- a call to retrieve Linda Duffy can go directly to machine 1 -- but can be slow if your query spans multiple machines. A query to get all users older than 18 will have to hit all three machines, resulting in slower responses.

Similarly, we saw in the previous section how strong consistency requirements can make it difficult to scale out. We would introduce latency during writes to make sure the write is committed to all nodes before returning to the writing user.

Relaxing these requirements makes it much easier for Dynamo to scale horizontally without sacrificing performance. DynamoDB uses [consistent hashing](https://en.wikipedia.org/wiki/Consistent_hashing) to spread items across a number of nodes. As the amount of data in your DynamoDB table increases, AWS can add additional nodes behind the scenes to handle this data.

DynamoDB avoids the multiple-machine problem by essentially requiring that all read operations use the primary key (other than Scans). From our Users example before, our primary key could be LastName, and Amazon would distribute the data accordingly. If you do need to query via Age, you would use a [secondary index](https://www.dynamodbguide.com/key-concepts#secondary-indexes) to apply the same distribution strategy via a different key.

Finally, because DynamoDB allows for eventual consistency, it allows for easier replication strategies of your data. You can have your item copied onto three different machines and query any of them for increased throughput. It's possible one of the machines has a slightly different view of the item at different times due to the eventual consistency model, but this is a trade-off worth accepting for many use cases. Also, you may explicitly specify a strongly-consistent read if it is required for your application.

These changes make it possible for DynamoDB to provide query latencies in single-digit milliseconds for virtually unlimited amounts of data -- 100TB+.



# Environment Setup

In many of the subsequent lessons, we'll be directly interacting with the AWS DynamoDB APIs. To do this, we'll need to set up our environment.

## Install the AWS CLI

The [AWS CLI](https://aws.amazon.com/cli/) is a nice command line utility for interacting with AWS services. 

```none
$ pip install awscli
```

If you have trouble installing it, [check the install instructions here](http://docs.aws.amazon.com/cli/latest/userguide/installing.html).

## Get IAM credentials

If you want to use a real AWS account, you'll need to set up your environment with the proper IAM credentials. You can read the AWS docs on doing that [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-quick-configuration).

The quickest route is to create an IAM profile with full DynamoDB permissions. The Policy statement would look like:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:*"
      ],
      "Resource": "*"
    }
  ]
}
```

Once you have the keys for your IAM user, you can add your profile with `aws configure`:

```bash
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

## (Optional) Use DynamoDB Local

AWS has a downloadable version of DynamoDB that you can run locally. This is ideal if you don't want to configure a real AWS account or if you want to avoid any AWS charges.

To use it, download the zip file and unzip it:

```bash
$ curl -O https://s3-us-west-2.amazonaws.com/dynamodb-local/dynamodb_local_latest.zip
$ unzip dynamodb_local_latest.zip
$ rm dynamodb_local_latest.zip
```

Then start your DynamoDB local instance:

```bash
$ java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb

Initializing DynamoDB Local with the following configuration:
Port:	8000
InMemory:	false
DbPath:	null
SharedDb:	true
shouldDelayTransientStatuses:	false
CorsParams:	*
```

If you see the initialization message in your terminal, you've successfully started the DynamoDB Local emulator. You're now ready to get started.

## The `$LOCAL` variable

If you want to use the DynamoDB local emulator, you'll need to append the following flag to all commands given in the examples:

```bash
--endpoint-url http://localhost:8000
```

I don't like typing the full flag every time so I export it to a variable and use that shorthand:

```bash
$ export LOCAL="--endpoint-url http://localhost:8000"

$ aws dynamodb list-tables $LOCAL
```

All examples will include the `$LOCAL` flag for easier copy-paste functionality. If you are using a real AWS account rather than the DynamoDB local emulator, make sure the `$LOCAL` variable is unset in your terminal.

# Anatomy of an Item

An *item* is the core unit of data in DynamoDB. It is comparable to a row in a relational database, a document in MongoDB, or a simple object in a programming language. Each item is *uniquely identifiable*by a primary key that is set on a table level.

An item is composed of *attributes*, which are bits of data on the item. This could be the "Name" for a User, or the "Year" for a Car. Attributes have types -- e.g., strings, numbers, lists, sets, etc -- which must be provided when writing and querying Items.

In this section, we'll cover the core aspects of Items, including:

- [Primary keys](https://www.dynamodbguide.com/anatomy-of-an-item#primary-keys),
- [Attributes](https://www.dynamodbguide.com/anatomy-of-an-item#attributes), and
- [Attribute types](https://www.dynamodbguide.com/anatomy-of-an-item#attribute-types).

## Primary Keys

> ***Every item in a table is uniquely identified by its primary key.\***

When creating a new table, you will need to specify the primary key of that table. Every item in a table is uniquely identified by its primary key. Accordingly, the primary key must be included with every item that is written to a DynamoDB table.

There are two types of primary keys. A **simple primary key** uses a single attribute to identify an item, such as a Username or an OrderId. Using a DynamoDB table with a simple primary key is similar to using most simple key-value stores, such as Memcached.

A **composite primary key** uses a combination of two attributes to identify a particular item. The first attribute is a *partition key* (also known as a "hash key") which is used to segment and distribute items across shards. The second attribute is a *sort key* (also known as a "range key") which is used to order items with the same partition key. A DynamoDB table with a composite primary key can use interesting query patterns beyond simple get / set operations.

Understanding the primary key is a crucial part of planning your data model for a DynamoDB table. The primary key will be your main method of inserting and updating items in your table.

## Attributes

An item is made up of *attributes*, which are different elements of data on a particular item. For example, an item in the User table might have a Name attribute, an Age attribute, an Address attribute, and more. They are comparable to columns in a relational database.

Most attributes in a DynamoDB table are *not* required for every item. DynamoDB is a NoSQL database which allows for a more flexible data model than the standard relational databases. You could store entirely different kinds of objects in a single DynamoDB table, such as a Car object with Make, Model, and Year attributes, and a Pet object with Type, Breed, Age, and Color attributes. This is a common practice, as you will often have multiple different entity types in a single table.

There is one exception to the flexible model of DynamoDB items -- each item **must** have the attribute(s) for the defined [primary key](https://www.dynamodbguide.com/anatomy-of-an-item#primary-keys) of the table.

## Attribute types

When setting an attribute for a DynamoDB item, you must specify the type of the attribute. Available types include simple types like strings and numbers as well as composite types like lists, maps, or sets.

When manipulating an item, you'll provide each attribute you're trying to set with the type of the attribute. This will be provided in a map where the keys are the names of the attributes to set. The values for each attribute is a map with a single key indicating the type of value for the attribute and the value being the actual value of the attribute.

For example, if you want to store a User object with three attributes of Name, Age, and Roles, your API call to store the User would look like:

```none
{
    "Name": { "S": "Alex DeBrie" },
    "Age": { "N": "29" },
    "Roles": { "L": [{ "S": "Admin" }, { "S": "User" }] }
}
```

In this example, we've stored a *string* attribute of "Name" with the value "Alex DeBrie" using the string indicator of "S". There's also a *number* attribute of "Age" with the value "29" with the number indicator of "N". Finally, there's a *list* attribute of "Roles" with the value containing two items, "Admin" and "User" using the list indicator of "L".

Similarly, when you retrieve an item from DynamoDB, it will return the attributes in a map with the attribute names as the keys of the map. The values of the map will be a map containing a single key indicating the type of the attribute and the value containing the actual value of the attribute.

For example, if you're using the GetItem API call to retrieve the User from above, your response would look like:

```none
{
    "Item": {
        "Name": {
            "S": "Alex DeBrie"
        },
        "Age": {
            "N": "29"
        },
        "Roles": {
            "L": [{ "S": "Admin" }, { "S": "User" }]
        }
    }
}
```

> Note that the value for the Age attribute is a string -- "29" -- rather than the actual number 29. In your application, you'll need to do conversions from a string type to a number type if needed.

With the basics of attribute types in mind, let's look at the different types of attributes. Each type starts with the identifier used (e.g. `S` for strings, `N` for numbers, etc) as well as an example usage.

#### String type

**Identifier:** "S"

**Example Usage:**

```none
"Name": { "S": "Alex DeBrie" }
```

The string type is the most basic data type, representing a Unicode string with UTF-8 encoding.

DynamoDB allows sorting and comparisons of string types according to the UTF-8 encoding. This can be helpful when sorting last names ("Give me all last names, sorted alphabetically") or when filtering ISO timestamps ("Give me all orders between "2017-07-01" and "2018-01-01").

#### Number type

**Identifier:** "N"

**Example Usage:**

```none
"Age": { "N": "29" }
```

The number type represents positive and negative numbers, or zero. It can be used for precision up to 38 digits.

Note that you will send your number up as a string value. However, you may do numerical operations on your number attributes when working with [condition expressions](https://www.dynamodbguide.com/expression-basics#condition-expressions).

#### Binary type

**Identifier:** "B"

**Example Usage:**

```none
"SecretMessage": { "B": "bXkgc3VwZXIgc2VjcmV0IHRleHQh" }
```

You can use DynamoDB to store Binary data directly, such as an image or compressed data. Generally, larger binary blobs should be stored in something like Amazon S3 rather than DynamoDB to enable greater throughput, but you may use DynamoDB if you like.

When using Binary data types, you must base64 encode your data before sending to DynamoDB.

#### Boolean type

**Identifier:** "BOOL"

**Example Usage:**

```none
"IsActive": { "BOOL": "false" }
```

The Boolean type stores either "true" or "false".

#### Null type

**Identifier:** "NULL"

**Example Usage:**

```none
"OrderId": { "NULL": "true" }
```

The Null type stores a boolean value of either "true" or "false". I would generally recommend against using it.

#### List type

**Identifier:** "L"

**Example Usage:**

```none
"Roles": { "L": [ "Admin", "User" ] }
```

The List type allows you to store a collection of values in a single attribute. The values are ordered and do not have to be of the same type (e.g. string or number).

You can operate directly on list elements using [expressions](https://www.dynamodbguide.com/expression-basics).

#### Map type

**Identifier:** "M"

**Example Usage:**

```none
"FamilyMembers": {
    "M": {
        "Bill Murray": {
            "Relationship": "Spouse",
            "Age": 65
        },
        "Tina Turner": {
            "Relationship": "Daughter",
            "Age": 78,
            "Occupation": "Singer"
        }
    }
}
```

Like the List type, the Map type allows you to store a collection of values in a single attribute. For a Map attribute, these values are stored in key-value pairs, similar to the map or dictionary objects in most programming languages.

Also like the List type, you can operate directly on map elements using [expressions](https://www.dynamodbguide.com/expression-basics).

#### String Set type

**Identifier:** "SS"

**Example Usage:**

```none
"Roles": { "SS": [ "Admin", "User" ] }
```

DynamoDB includes three different Set types which allow you to maintain a collection of *unique* items of the same type. The String Set is used to hold a set of strings.

Sets can be particularly useful with [expressions](https://www.dynamodbguide.com/expression-basics). You can run update commands to add & remove elements to a set without fetching & inserting the whole object. You may also check for the existence of an element within a set when updating or retrieving items.

#### Number Set type

**Identifier:** "NS"

**Example Usage:**

```none
"RelatedUsers": { "NS": [ "123", "456", "789" ] }
```

DynamoDB includes three different Set types which allow you to maintain a collection of *unique* items of the same type. The Number Set is used to hold a set of numbers.

Sets can be particularly useful with [expressions](https://www.dynamodbguide.com/expression-basics). You can run update commands to add & remove elements to a set without fetching & inserting the whole object. You may also check for the existence of an element within a set when updating or retrieving items.

#### Binary Set type

**Identifier:** "BS"

**Example Usage:**

```none
"SecretCodes": { "BS": [ 
	"c2VjcmV0IG1lc3NhZ2UgMQ==", 
	"YW5vdGhlciBzZWNyZXQ=", 
	"dGhpcmQgc2VjcmV0" 
] }
```

DynamoDB includes three different Set types which allow you to maintain a collection of *unique* items of the same type. The Binary Set is used to hold a set of binary values.

Sets can be particularly useful with [expressions](https://www.dynamodbguide.com/expression-basics). You can run update commands to add & remove elements to a set without fetching & inserting the whole object. You may also check for the existence of an element within a set when updating or retrieving items.