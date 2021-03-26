#### What is app scaling? 
- Scalability is the function of your app to manage an increasing number of customers, clients and/or users. 
- Clients Per Second (all requests - get + post + put at the same time per second) vs RPS (one request per second)
- To know whether your app is scalabe or not is to test it -> scalability testing 
- The main idea behind scalability testing is to determine the major workloads and mitigate bottlenecks that can impede your app's scalability. 
- For each request measure:
    - RPS (Request Per Second)
    - Latency (Response Time) 
    - Error Rate
    - Throughput

   ![Screen Shot 2021-03-26 at 4 54 49 PM](https://user-images.githubusercontent.com/5890251/112703395-022f8d80-8e54-11eb-83c1-a0817b3b2864.png)

    
   ![Screen Shot 2021-03-26 at 4 50 52 PM](https://user-images.githubusercontent.com/5890251/112703253-70278500-8e53-11eb-9246-97ce1994c5e1.png)
   
##### Scaling: Development environment

```Smoke testing -> regular load test, configured for minimal load. Run this as a sanity check
  - to verify that our system doesn't throw any error under minimal load. ```

TEST: 

ANALYZE: 

CHANGE: 

##### Scaling: Production environment using loader.io (cloud based testing environment)

TEST

ANALYZE

TEST 
