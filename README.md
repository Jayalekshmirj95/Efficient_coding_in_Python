# Points to remember to improve the efficiency of python programs
Hi, I am going to take you through some of the things you can simply do to increase the efficiency of your python program. As you read through this, you will understand these are all basic things we often ignore or forget to implement even though we are aware of them.\

Let me first explain a little bit about the project that we are working on. So PDQ is a company that has two software products- PDQ Deploy & PDQ Inventory. They sell licenses to their customers to use these products for the alloacted time. Our task is to create a table which contains all the historical invoices starting from January 2017. We have around 7 source tables that we can use to create the resultant table called Invoices. BigQuery is the platform where PDQ has stored these tables for now. From there we can get the data using Python to connect with BigQuery and then do the processing and manipulations to get the final output.


