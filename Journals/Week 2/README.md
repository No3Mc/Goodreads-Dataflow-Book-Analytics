**LAB 02 - CLASSIFIED ADVERTISEMENT DATABASE**

**OPERATIONAL DETAILS**

Try out all examples and exercises. It is a good idea to keep a copy of your solutions in a text file using
a text editor so you can reuse them for future exercises and revision.

**SCENARIO**

A business enterprise requires you to build a database to store the details of classified advertisements.
Different products are for sale and these need to advertise different attributes. For example, cars have
model and mileage while clothes have size and brand, etc.

This is not easy to model for a relational database. One option would be to create one large table with
all the fields such as:

**ClassifiedAdvert(ID, model, mileage, size, brand, price, ...)**

However, it is very difficult to predict every attribute for every possible product, and for each product,
most fields will be null.


A second option would be to create a table for each classification of product such as:


    MotorCar(ID, model, mileage, price, ...)
    Clothing(ID, size, brand, price, ...)

However, in this case, every new classification would need a new table created and new queries to
work with the new table.

A third option would be to create very general tables to store the attributes as values, such as:

    ClassifiedAdvert(AdID, classification, ...)
    ClassifiedAdvertAttribute(AdAttrID, attributeName, value, AdID)

However, you would need an Attribute table for each datatype to store every possible value and this
would require many joins to advertise a product.


It is much easier to model this as a collection of documents in MongoDB. Each advertisement would be a document to represent the product with whatever attributes it needs. 

For example:

    {
    "Manufacturer":"Fiat",
    "Colour":"Red",
    "Model":"Panda",
    "Milage":31406.0,
    "Price":1843.76,
    "Classification":"Motor Cars"
    },
    {
    "Clothing Type":"Trousers",
    "Colour":"Green",
    "Size":"Long",
    "Price":49.7,
    "Classification":"Clothes"
    },
    {

**EXERCISE 1**

CREATE DATABASE & COLLECTION
In MongoDB Compass, create a new database ClassifiedAds and a new collection named ads to store
classified advertisements.

**IMPORT DOCUMENTS**

Download two text files of data from the Week 02 page on Blackboard. One contains documents about
motor cars and the other about clothing. They are motorcars.txt and clothing.txt respectively.

In MongoDB Compass,

1. click Add Data -> Import File.

2. Click on Browse and locate motorcars.txt

3. Select CSV option .

4. Then click Import

Do the same for clothing.txt


**EXERCISE 2**

1. Write the following basic MongoDB queries against the new collection:

a. Retrieve all the Motor Cars for sale.

b. Retrieve all the Motor Cars with prices under £1500.

c. Retrieve only the prices of all the Red Fiats.

2. Aggregate data from the Collection
We can use single purpose aggregation functions to count the number of documents in a
collection or remove duplicates from a result.

a. Let’s count the total number of classifieds in the collection:

b. Next retrieve the set of all models of motor cars, but making sure that each model appears
only once in the result:

**Exercise 3**

1. How many motor car classifications are there?

2. Retrieve the set of all Classifications. Make sure each classification appears only once in the
result.

3. Say we wanted to find the lowest mileage of all the motor cars. It is possible to do this in three
ways:

a. We could find the mileage by sorting and limiting the returned results to one
document:

b. The second method uses an aggregation pipeline to match motor cars and then group
them by classification to calculate the minimum mileage:

**Exercise 4**

Using the two different methods (if possible)

1. Find the most expensive item of clothing.

2. The data does not tell us whether an object has been sold or not. Write a query to update the
database so that the Red Shirt with a price of 20.31 has been sold [hint: create a new field].

3. Now write a query to find out how many items of Clothing have not been sold [hint: $ne].

https://docs.mongodb.com/manual/reference/operator/query/ne/

**Exercise 5**

The company wants to invest in a new Digital billboard that views Red coloured ads efficiently.

1. Find how many ads are Red in colour to assist them in making the decision

2. What is the percentage of Red ads in the entire collection

3. How much revenue could the company get for every Clothing type?

**Exercise 6**

As a new regulation and in line with best practices, Cars with high Mileage will be more expensive to
advertise.

1. How many Cars have Mileage above 50,000?

2. Create a new field "UpdatedPrice" that has 5% extra price to all cars with mileage above
50,000.

1. Verify that the update is done successfully