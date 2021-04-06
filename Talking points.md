Feedback
- optmized version, cutting some details might have been better 
- lot of ums and ahhs 
- keep your tone consistent 
- why tinker with nginx, 


#### Intro
- Recently I worked on a project where I built the backend infrastructure for an e-commerce website similar to adidas. 
- I inherited a legacy codebase with front end already built out.
- My main task was to build the backend infrastructure for reviews microservice with primary focus on system design and scaling. 

#### Technologies used
- Technologies that I used were express, node and MySQL for database. 
- I handled data that consisted of 1 Million Product ids, 5 Million Reviews with 2 Million Photos and around 20 Million characteristics meta data. 
- The reason I went with MySQL is because my data is fairly structured with two many to many relationship between different entities, and MySQL provides an easy way to work with structured data. 

##### Goals 
- My overall goal was to get my application to handle throughput of 1000 RPS in the production environment, with response time or latency of 2000 ms under load and error rate of less than 1%.  

#### Deployment 
-  After achieving my sub goal of query time of less than 50 ms in the local environment, I decided to deploy my microservice and MySQL database onto the cloud using AWS EC2 instance. 

#### Observations and bottlenecks 
Using loader.io integrated with new relic testing platform, I observed that my response time was was pretty high (5 seconds) for 500 RPS. 
And AWS cloudwatch report showed that my CPU utilization of MySQL+Reviews EC2 instance was around 30%, 
- so to identify the bottleneck, first I deployed MySQL and microservice separately. 
- This showed that the CPU utilization was still around 30% for mySQL database. very high compared to my microservice instance. 

#### First optimization: (Query Optimization) indexing and inner join (5 to 3 seconds)
Now that I knew that my bottleneck was in the database level, 
- Just to provide some context, I had two GET requests. 
- my core query searches the reviews table for all the reviews and photos table for all the photos related to that reviews
- my second query searches three different tables to find characteristic name, value, and id for all the reviews of a specific product id. (first queries the join table of char and product to get the characteristic id for that product and using that information to find all the charactertistics name, value and id attached to all the reviews for that specific product )
- I knew that I had room for optimization here, and I could use inner joins to make my query more efficient. 
- I replaced my nested queries (multiple callbacks) with inner join. 
- I also added indexes on tables with join columns (characteristics 
- This reduced my query time to 3 seconds. 
 
#### Second Optimization: Load Balancer Optimization: 
3 seconds is still too high and there was more room for optimization so I decided to scale my application horizontally using nginx load balancer. 
- I added a proxy server and scaled up my microservices to two instances which then communicated with 1 Database EC2 instance. 
- I placed a nginx Load Balancer in between the proxy server and my microservice instances using Least connection strategy. I explored ip hashing and round robin and my results were pretty consistent. 
- Two microservice servers then interacted with one database EC2 instance. 
- The exciting part was that this reduced my repsonse time from 3 seconds to 900 ms on throughput of 1000RPS. 
- (623ms with 8.4% error rate on throughput of 1000 clients over 1 minute)


#### interesting topics to talk about in order
- horizontal scaling: nginx load balancing with least-conn strategy 
- separation of servers and database using docker 
- indexing 
- error rate is 8% don't talk about it 


---------------------------------------------------------------------------------------------------------------------------

#### What does my microservice do: 
- User should be able to write new reviews, 
- read all the reviews, view the ratings  for the product they choose
- sort it based on different categories such as helpfulness, relevant 

#### How much data and organization and what database
To store my data after cleaning using nodestream, I went with a relational database, MySQL. I created about 6 tables to store my data and organized the data using foreign keys 
- 1M Products with 5 M Reviews 
- 2M Photos 
- 1M Characteristics
- 3M Char_Product
- 19M Char_Reviews

#### Why MySQL
The reason I went with MySQL is because my data is fairly structured with two many to many relationship between different entities, and MySQL provides an easy way to work with structured data. 

#### Core queries 
I had two GET requests. 
- my core query searches the reviews table for all the reviews and photos table for all the photos related to that reviews
- my second query searches three different tables to find characteristic name, value, and id for all the reviews of a specific product id. (first queries the join table of char and product to get the characteristic id for that product and using that information to find all the charactertistics name, value and id attached to all the reviews for that specific product )



#### Third Optimization: (maybe don't talk about it during an interview or make it more robust then talk about it) Redis Caching at the microservice layer with Time To Live of 600.
- because redis is a in-memory data store, memory access is faster than disk centric databases.
- schema-less key-value NoSQL type data-store
- cache hit ratio of 85% which is still way to high. 
- lazy-caching strategy with some logic in my server file to first re-direct the get request from client to cache and if it's a hit, then return that data and if it's a miss then get the data from the database and also update the cache in the process. 
- 623ms with 8.4% error rate on throughput of 1000 clients over 1 minute 

![Screen Shot 2021-03-30 at 3 51 13 PM](https://user-images.githubusercontent.com/5890251/113066676-cbb58380-916f-11eb-8aab-875770bf8835.png)

There is definitely room for optimization. Cache hit ration is pretty high
- it's reactive vs caching by pre-fetching the most popular reviews and saving them 
- I could make my caching more robust. 
- 
