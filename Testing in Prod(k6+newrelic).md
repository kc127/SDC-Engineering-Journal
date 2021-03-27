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

``` Smoke testing -> regular load test, configured for minimal load. Run this as a sanity check to verify that our system doesn't throw any error under minimal load. ```

TEST: 1 user looping for 1 minute, 10 users/s, 100 users/s, 1K users/s (last 10% of my database)

![Screen Shot 2021-03-26 at 5 02 34 PM](https://user-images.githubusercontent.com/5890251/112703682-132cce80-8e55-11eb-9aaf-26a75450c2a8.png)

ANALYZE: 0.0% Error rate 

CHANGE: No change

```Load Testing: primarily concerned with assessing the current performance of my app in terms of concurrent users or requests per second. To understand whether my system is meeting performance goals, load testing is done. To determine system's behavior under both normal and peak conditions by configuring perfomance thresholds in k6```

TEST: (Load-test.js) 100 users over 1 minutes, stay at 100 users for 10 minutes, ramp down to 0 users. 
      (ramp-up-test.js) 
      
![Screen Shot 2021-03-26 at 5 26 09 PM](https://user-images.githubusercontent.com/5890251/112704583-605e6f80-8e58-11eb-9ba7-d36d64e50692.png)

(load-test.js)

![Screen Shot 2021-03-26 at 5 30 18 PM](https://user-images.githubusercontent.com/5890251/112704745-fb574980-8e58-11eb-8104-43339ce502f5.png)

(ramp-up.test.js)

<img width="844" alt="Screen Shot 2021-03-26 at 6 11 21 PM" src="https://user-images.githubusercontent.com/5890251/112706023-bdf5ba80-8e5e-11eb-9438-bea4c8d66a70.png">


ANALYZE:  I noticed that 99% of the requests completed below 1.5 seconds in k6. New relic is showing that my response time is 26 ms. Error rate was 0.0%, throughput was 6040 rpm)

(new relic so far) 

<img width="1174" alt="Screen Shot 2021-03-26 at 6 00 24 PM" src="https://user-images.githubusercontent.com/5890251/112705827-a9fd8900-8e5d-11eb-8c5e-1b520bb445c2.png">

<img width="1165" alt="Screen Shot 2021-03-26 at 6 00 35 PM" src="https://user-images.githubusercontent.com/5890251/112705832-ad911000-8e5d-11eb-93d7-36d983241ef9.png">

At the peak, the response time is 

<img width="765" alt="Screen Shot 2021-03-26 at 6 12 59 PM" src="https://user-images.githubusercontent.com/5890251/112706062-02815600-8e5f-11eb-9e9a-eedc831b513a.png">


CHANGE: None



``` Stress Test : many different type of load testing, if your system crashes under a load test, it means that my load test has morphed into stress test. ```
  [] how your system behaves under extreme conditions
  [] max capacity of your system in terms of users or throughput
  [] breaking point of your system and its failure mode
  [] determine if you system will recover without manual intervention after the stress test is over

To push my system over its normal operations, to its limits, and beyond the breaking point, (don't overwhelm the system, it will turn into spike test), I started with the follow configuration:

TEST :
<img width="474" alt="Screen Shot 2021-03-26 at 6 16 55 PM" src="https://user-images.githubusercontent.com/5890251/112706133-76236300-8e5f-11eb-9d91-ef97f7b86ec8.png">

This test increases the load by 100 users every 2 minutes and stays at this level for 5 minutes. Recovery stage is also included where the system is gradually decreasing the load to 0. 

![Screen Shot 2021-03-26 at 7 25 09 PM](https://user-images.githubusercontent.com/5890251/112707486-1df15e80-8e69-11eb-926f-36802b3706b7.png)


ANALYSE: 

CHANGE: 

```Spike Testing: variarion of stress test, spikes to extreme load over a very short window of time , immediately overwhelms system with an extreme surge of load. You want to spike test to see if you system can perform under sudden surge of traffic, if your system will recover once the traffic has subsided```

TEST:

ANALYSE: 

<img width="569" alt="Screen Shot 2021-03-26 at 6 22 17 PM" src="https://user-images.githubusercontent.com/5890251/112706230-34df8300-8e60-11eb-9dcf-48fe6aabc9c8.png">

