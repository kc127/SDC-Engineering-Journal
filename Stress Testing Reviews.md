 
API-Server -> Load Balancer -> Reviews_API_1, Reviews_API_2 -> MySQL

```
mysql> SELECT MAX(id) FROM product;
+---------+
| MAX(id) |
+---------+
| 1000011 |
+---------+
1 row in set (0.01 sec)

mysql> SELECT MAX(id) FROM photos;
+---------+
| MAX(id) |
+---------+
| 2742932 |
+---------+
1 row in set (0.01 sec)

mysql> SELECT MAX(id) FROM reviews;
+---------+
| MAX(id) |
+---------+
| 5777993 |
+---------+
1 row in set (0.00 sec)

mysql> SELECT MAX(id) FROM characteristics;
+---------+
| MAX(id) |
+---------+
| 3347582 |
+---------+
1 row in set (0.00 sec)

mysql> SELECT MAX(id) FROM characteristics_product;
+---------+
| MAX(id) |
+---------+
| 3347492 |
+---------+
1 row in set (0.00 sec)

mysql> SELECT MAX(id) FROM characteristics_reviews;
+----------+
| MAX(id)  |
+----------+
| 19337419 |
+----------+
```


#### 1.Stress Test (500 clients per second over 1 minute => 500 * 60 = 30,000 Requests)


Response Time (Latency): 3.5 s

Error Rate: 15.9%

Throughput(Total Successful Requests): 22,201 Requests

Bottleneck: Based on AWS Cloud Watch report, CPU utilization of both of both microservice is pretty high, with high traffic as potential cause:

![Screen Shot 2021-03-28 at 7 51 03 PM](https://user-images.githubusercontent.com/5890251/112781301-63e02b00-8fff-11eb-9461-a677739fa946.png)


Possible Optimization: Indexing Reviews table, De-normalization/normalization, Caching, Data Partitioning

![Screen Shot 2021-03-28 at 6 09 39 PM](https://user-images.githubusercontent.com/5890251/112775491-e104a380-8ff1-11eb-921f-419aeec6b246.png)

#### 2. Stress Test (1000 clients per second over 1 minute => 1000 * 60 = 60,000 Requests)

Response Time (Latency): 6.1 second
Error rate: 30%
Throughput(Total Successful Requests): 22,000 Requests
Bottleneck: Same as above
Possible Optimization: same as above

![Screen Shot 2021-03-28 at 6 09 39 PM](https://user-images.githubusercontent.com/5890251/112775800-c979ea80-8ff2-11eb-946f-e84bbec1edaa.png)

#### Optimization: 

### 1. Data Partitioning (couldn't figure out due to foreign key constraints)
ALTER table characteristics_reviews 
  PARTITION BY HASH(id) 
  PARTITIONS 4;
  
  ALTER TABLE characteristics_reviews  PARTITION BY RANGE (id)
  (
    PARTITION p1 VALUES LESS THAN (5000,000),
    PARTITION p2 VALUES LESS THAN (10,000,000), 
    PARTITION p3 VALUES LESS THAN (15,000,000),  
    PARTITION p4 VALUES LESS THAN (20,000,000)
  );
  
### 2. Indexing
```ALTER TABLE photos ADD INDEX new_reviews_id (reviews_id);```

```ALTER TABLE characteristics_reviews ADD INDEX char_reviews_id (reviews_id);```

```ALTER TABLE characteristics_reviews ADD INDEX char_chars_id (characteristics_id);```

```ALTER TABLE characteristics_product ADD INDEX char_product_id (product_id);```


<img width="661" alt="Screen Shot 2021-03-28 at 9 04 14 PM" src="https://user-images.githubusercontent.com/5890251/112785574-297b8b80-9009-11eb-9d90-7d5fab8b3db9.png">


Effect after partitioning (same params as before)

![Screen Shot 2021-03-28 at 9 06 24 PM](https://user-images.githubusercontent.com/5890251/112785697-78c1bc00-9009-11eb-8084-0994d56644da.png)

### 3. Redis Caching - I used on-demand cache

 Source (https://livecodestream.dev/post/beginners-guide-to-redis-and-caching-with-nodejs/)
 Redis Set-up on Ubuntu: https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04
 
 I ended up using the following design. I hosted redis within the same EC2 instance as my Reviews_API servers. 
 
 https://redis.io/topics/client-side-caching
 
 ![Screen Shot 2021-03-29 at 12 02 40 AM](https://user-images.githubusercontent.com/5890251/112801143-65bce500-9025-11eb-8168-8458e8371204.png)

 
 ![Screen Shot 2021-03-28 at 11 50 27 PM](https://user-images.githubusercontent.com/5890251/112797564-6dc65600-9020-11eb-85de-def2a31ade8b.png)

![Screen Shot 2021-03-29 at 12 24 22 AM](https://user-images.githubusercontent.com/5890251/112801050-48881680-9025-11eb-8dfa-25a4a258ee92.png)

![Screen Shot 2021-03-29 at 12 14 05 AM](https://user-images.githubusercontent.com/5890251/112801058-4c1b9d80-9025-11eb-9d86-b80ab6c4d705.png)

![Screen Shot 2021-03-29 at 12 06 10 AM](https://user-images.githubusercontent.com/5890251/112801102-589ff600-9025-11eb-9840-7e3f75d4de95.png)

![Screen Shot 2021-03-29 at 12 01 08 AM](https://user-images.githubusercontent.com/5890251/112801189-6fdee380-9025-11eb-95f4-e87e185721a2.png)

