# How to improve the efficiency of your python program
Hi, I am going to take you through some of the things you can simply do to increase the efficiency of your python program. As you read through this, you will understand these are all basic things we often ignore or forget to implement even though we are aware of them.

Let me first explain a little bit about the project that we are working on. So PDQ is a company that has two software products- PDQ Deploy & PDQ Inventory. They sell licenses to their customers to use these products for the alloacted time. Our task is to create a table which contains all the historical invoices starting from January 2017. We have around 7 source tables that we can use to create the resultant table called Invoices. BigQuery is the platform where PDQ has stored these tables for now. From there we can get the data using Python to connect with BigQuery and then do the processing and manipulations to get the final output.

## How did we find out the code execution time is becoming a problem
So this is how we implemented this usecase. We first connect with bigquery in Python, and do some queries to pull the data out from bigquery database and then do the manipulations using Pandas dataframe operations. As part of the task, we had to join different tables and did the joining conditions in Python too which at the end I realized was a mistake.\
The program we write has to run for around 70000 customers, which means we have to generate invoices for these many customers. But first we can try one customer and if that is a success, we can scale the code. With that in mind, we ran the code for one customer and the execution time was around 60 minutes which absolutely is really bad. 
So I tried some things and was able to reduce the execution time to just around 50 seconds for one customer, which is still not perfect but something. Before going into what did I do to achieve this I think it is worth mentioning how we can find the execution time of the code.

There are many modules using which you can find the execution time of the code. The one that I used is the _time_ module. A sample code and its output are shown below:\
<img src="https://user-images.githubusercontent.com/82940730/152287999-523ea722-f858-4d11-84ba-76488be77088.png" width="400" height="200"/>
_time.time()_ gives you the time in seconds since epoch. The execution time here is 0.0006 seconds.

## How did we reduce the execution time of the project code
This was more of a _trial and achieve_ approach. We tried multiple things which I am going to list down. A word of caution, some of these may not necessarily work for your program, but there is no harm in trying them.
### 1. Tuples are faster than lists
Tuples are stored in a single block of memory. Tuples are immutable so, It doesn't require extra space to store new objects. But in the case of lists, they are allocated in two blocks: the fixed one with all the Python object information and a variable sized block for the data. This is why tuples are faster than lists. So if you do not need to modify the elements afterwards it's better to go with tuples as a choice. 
### 2. Multiple assignments
When you want to initialize multiple variables try something like this in a single line:\
_a, b, c = 10, 20, 30_
### 3. List comprehension
You do not want to use any other technique if there is a possibility of using list comprehension. An example is below:\
<img src="https://user-images.githubusercontent.com/82940730/152291791-1bc87a07-7484-46a8-9703-ad5185fddfdc.png" width="400" height="250"/>
You can see, the execution time has been reduced. It was just a simple program but think of what can it do for a very long program of say 2000 lines of code.
### 4. Use joins when concatenating strings
If you have some strings to be concatenated, the best way to do it is to store the strings inside a list and then use the _join()_ method to concatenate them. An example is given below:\
<img src= "https://user-images.githubusercontent.com/82940730/152732113-11b91298-314f-4dad-8199-0b3e7160fbe1.png" width='400' height='250'/>
_join()_ is faster than + operation because + operator creates a new string and copies the content at each step but _join()_ does not do that.
### 5. Avoid dot operations
We cannot avoid importing modules in Python, but what we can avoid is, importing the whole module(root module). For example, instead of importing math module, we can just import the required function from math module. Example below:\
<img src= "https://user-images.githubusercontent.com/82940730/152732692-213f8f0c-e8dc-4085-9552-cb9ffca614c0.png" width='400' height='250'/>
### 6. Avoid loops
Loops in python are time consuming, especially for loops. Try to avoid them. We can use _map()_ in some cases, example below:\
<img src ="https://user-images.githubusercontent.com/82940730/152735356-809d79d8-90d3-4197-9d8d-ea1ef1583632.png" width='400' height='250'/>\
The _map()_ returns a list of the results after applying the given function to each item of a given iterable (list, tuple etc.)\
Also it is worth trying while loops instead of for loops to reduce the execution time.
### 7. Pandas: iterrows()
Pandas is one of the famous Python libraries, that are very efficient. But sometimes when it comes to larger datasets, it becomes slower to iterate over the records. There are some things we can try to make it more performant.\
The _iterrows()_ is used to iterate over a pandas Data frame rows in the form of (index, series) pair. But this function is not recommended to use as it is slower and it does not preserve dtypes across the rows (dtypes are preserved across columns for DataFrames). Also you should never modify something you are iterating over. Depending on the data types, the iterator returns a copy and not a view, and writing to it will have no effect.\
<img src='https://user-images.githubusercontent.com/82940730/152775678-3bf84598-434a-4c0c-a786-197115ef805d.png' width='400' height='250'/>\
Instead of using _iterrows()_, we have more efficient way to iterate over the rows of the dataframe which is using index.
### 8. Pandas: Iteration by index
The rows and columns of the data frame are indexed, and you can loop over the indexes to iterate through the rows. An example is given below:\
<img src="https://user-images.githubusercontent.com/82940730/152778425-372382eb-1f02-44b2-9392-17d5fe8569d3.png" width='400' height='250'/>\
The _df.shape()_ stores the number of rows and columns of a pandas dataframe as a tuple.
### 9. Pandas: to_dict()
You can use _to_dict()_ function in Pandas to convert the data frame to a dictionary. Iterating over a dictionary is comparatively very fast compared to _iterrows()_ function.
Let's look at an example:\
<img src="https://user-images.githubusercontent.com/82940730/152781158-478953b0-bc4c-4d88-a9a9-e2c90fc76edf.png" width='400' height='500'/>
### 10. Pandas: apply()
It is a built-in function which can effectively be used to apply a function to each value of a pandas series.
<img src="https://user-images.githubusercontent.com/82940730/152782631-d8ad55c8-824f-4d8c-b8a9-f28981cba792.png" width='400' height='250'/>\
Here you are able to modify a column of the dataframe without even using a for loop. You can write complex functions and then apply it to each values of the series using _apply()_. This function is known as one of the most powerful python functions. Lambda() is another one. Using _apply()_ we were able to reduce the execution time of the code to a great extend.\
### 11. Pandas: Swifter
A package which efficiently applies any function to a pandas dataframe or series in the fastest available manner. You can install this package using the below line:\
_pip install swifter_\
After installing you can use it in the program to increase the efficiency of your code. A sample code is given below:\
<img src="https://user-images.githubusercontent.com/82940730/152802439-11ea7266-e5fc-4fce-a3de-e220e8b704d0.png" width='600' height='250'/>\
And the output is given below:\
<img src="https://user-images.githubusercontent.com/82940730/152802779-5d478469-4cc1-4938-bd0d-b392b0043278.png" width='400' height='50'/>\
You can see the execution time has been reduced (the difference here is very small because I used a dataset which has only around 5k rows).
### 12. Pandas: Vectorization
We are getting to the last and most important part of this article, vectorization. In mathematics, a vector is something that has magnitude and direction. In programming and computer science, vectorization is the process of applying operations to an entire set of values at once. Vectorization is used to speed up the Python code without using loops. Using such a function can help in minimizing the running time of code efficiently. The built-in methods in NumPy and Pandas are built with C, which allows for vectorization. Vectorization almost always works faster as execution time is either constant, or grows at a much slower rate with a larger number of elements. Let's look at an example:\
<img src= "https://user-images.githubusercontent.com/82940730/155304285-047b7c82-2905-4b0b-aef3-1037dfc00b5b.png" width='600' height='250'/>\
Here line number 11 shows one form of vectorization. The output is as below:\
<img src="https://user-images.githubusercontent.com/82940730/155304456-581c2bcd-5109-4a19-81b3-bbae550a1fd1.png" width='400' height='50'/>\
There are a lot of other ways to do it more efficiently.\
We have _numpy.where()_ which can be used effectively to vectorize if-else conditions. Syntax given below:\
_numpy.where(condition, value/operation if true, value/operation if false)_\
This is an example:\
<img src="https://user-images.githubusercontent.com/82940730/155305796-769b594e-6c46-420f-8bf9-33cccfbe808a.png" width='700' height='50'/>\
If you have multiple if-else conditions in your function, it is always possible to use multilple _numpy.where()_ functions in your code. _numpy.select()_ is another function which comes in handy. An example below:\
<img src='https://user-images.githubusercontent.com/82940730/155307981-bac820c2-c270-4eeb-8a67-4f55006168a7.png' width='600' height='250'/>

































