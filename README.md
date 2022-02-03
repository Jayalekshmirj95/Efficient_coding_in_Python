# How to improve the efficiency of your python program
Hi, I am going to take you through some of the things you can simply do to increase the efficiency of your python program. As you read through this, you will understand these are all basic things we often ignore or forget to implement even though we are aware of them.

Let me first explain a little bit about the project that we are working on. So PDQ is a company that has two software products- PDQ Deploy & PDQ Inventory. They sell licenses to their customers to use these products for the alloacted time. Our task is to create a table which contains all the historical invoices starting from January 2017. We have around 7 source tables that we can use to create the resultant table called Invoices. BigQuery is the platform where PDQ has stored these tables for now. From there we can get the data using Python to connect with BigQuery and then do the processing and manipulations to get the final output.

## How did we find out the code execution time is becoming a problem
So this is how we implemented this usecase. We first connect with bigquery in Python, and do some queries to pull the data out from bigquery database and then do the manipulations using Pandas dataframe operations. As part of the task, we had to join different tables and did the joining conditions in Python too which at the end I realized was a mistake.\
The program we write has to run for around 70000 customers, which means we have to generate invoices for these many customers. But first we can try one customer and if that is a success, we can scale the code. With that in mind, we ran the code for one customer and the execution time was around 60 minutes which absolutely is really bad. 
So I tried some things and was able to reduce the execution time to just around 50 seconds for one customer, which is still not perfect but something. Before going into what did I do to achieve this I think it is worth mentioning how we can find the execution time of the code.

There are many modules using which you can find the execution time of the code. The one that I used is the _time_ module. A sample code and its output are shown below:\
<img src="https://user-images.githubusercontent.com/82940730/152287999-523ea722-f858-4d11-84ba-76488be77088.png" width="800" height="250">\
_time.time()_ gives you the time in seconds since epoch. The execution time here is 0.0006 seconds.

## How did we reduce the execution time of the project code
This was more of a _trial and achieve_ approach. We tried multiple things which I am going to list down. A word of caution, some of these may not necessarily work for your program, but there is no harm in trying them.
1. Tuples are faster than lists\
Tuples are stored in a single block of memory. Tuples are immutable so, It doesn't require extra space to store new objects. But in the case of lists, they are allocated in two blocks: the fixed one with all the Python object information and a variable sized block for the data. This is why tuples are faster than lists. So if you do not need to modify the elements afterwards it's better to go with tuples as a choice. 
2. Multiple assignments\
When you want to initialize multiple variables try something like this in a single line:\
_a, b, c = 10, 20, 30_
3. List comprehension\
You do not want to use any other technique if there is a possibility of using list comprehension. An example is below:\
<img src="https://user-images.githubusercontent.com/82940730/152291791-1bc87a07-7484-46a8-9703-ad5185fddfdc.png" width="800" height="400">\
You can see the execution time has been reduced, it was just a simple program but think of what can it do for a long program of say 2000 lines of code.































