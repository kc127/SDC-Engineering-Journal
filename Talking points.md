Hi, my name is Kanchan, I built the reviews microservice for a retail web-portal that handles viewing and submission of reviews for the product selected. 

What does my microservice do: 
- User should be able to write new reviews, 
- read all the reviews, view the ratings  for the product they choose
- sort it based on different categories such as helpfulness, relevant 

To store my data after cleaning using nodestream, I went with MySQL. I created about 6 tables to store my data using MySQL. 
- 1M Products with 5 M Reviews 
- 2M Photos 
- 1M Characteristics
- 3M Char_Product
- 19M Char_Reviews

The reason I went with MySQL is because my data is fairly structured with two many to many relationship between different entities, and MySQL provides an easy way to work with structured data. 

I had two GET requests. 
- my core query searches the reviews table for all the reviews and photos table for all the photos related to that reviews
- my second query first queries the join table of char and product to get the characteristic id for that product and using that information to find all the charactertistics name, value and id attached to all the reviews for that specific product 

I integrated new relic with k6 and loader.io and was able to determine that my database query time was pretty high
Also cloudwatch report showed that my CPU utilization of MySQL+Reviews EC2 instance was around 30%, 
- so To identify the bottleneck, first I deployed MySQL and microservice separately. 
- This showed that the CPU utilization was still around 30% for mySQL database. very high compared to my microservice instance. 
- With no indexes and without nested queries,  the query took 5 seconds 
- Therefore I indexed my tables with index on tables with join columns and nested queries, the query time went down to 3 seconds
 

Load Balancer Optimization: 3 seconds is still too high so I decided to scale horizontally  
- response time went down by 2 seconds 

Redis Caching at the microservice layer with Time To Live of 600.
- because redis is a in-memory data store, memory access is faster than disk centric databases.
- schema-less key-value NoSQL type data-store
- cache hit ratio of 85% which is still way to high. 
- lazy-caching strategy with some logic in my server file to first re-direct the get request from client to cache and if it's a hit, then return that data and if it's a miss then get the data from the database and also update the cache in the process. 
- 623ms with 8.4% error rate on throughput of 1000 clients over 1 minute 

There is definitely room for optimization. Cache hit ration i
- I could make my caching more robust. 
