API-Server -> Load Balancer -> 

#### 1.Stress Test (500 clients per second over 1 minute => 500 * 60 = 30,000 Requests)


Response Time (Latency): 3.5 s
Error Rate: 15.9%
Throughput(Total Successful Requests): 22,201 Requests
Bottleneck: To find the bottleneck, I am using mysql query optimizer 
Possible Optimization: Indexing Reviews table, De-normalization, Caching, increasing r

![Screen Shot 2021-03-28 at 6 09 39 PM](https://user-images.githubusercontent.com/5890251/112775491-e104a380-8ff1-11eb-921f-419aeec6b246.png)

#### 2. Stress Test (1000 clients per second over 1 minute => 1000 * 60 = 60,000 Requests)

Response Time (Latency): 6.1 second
Error rate: 30%
Throughput(Total Successful Requests): 22,000 Requests
Bottleneck: 
Possible Optimization: 

![Screen Shot 2021-03-28 at 6 09 39 PM](https://user-images.githubusercontent.com/5890251/112775800-c979ea80-8ff2-11eb-946f-e84bbec1edaa.png)
