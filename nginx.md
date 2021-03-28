### Deploying NGINX based http/https Load Balancer with ec2

#### Setting up NGINX as a Web Server and Reverse Proxy in AWS 

![Screen Shot 2021-03-26 at 11 34 25 PM](https://user-images.githubusercontent.com/5890251/112712321-0c6d7e00-8e8c-11eb-8305-9560476471a2.png)
![Screen Shot 2021-03-26 at 11 34 42 PM](https://user-images.githubusercontent.com/5890251/112712323-0d9eab00-8e8c-11eb-8a8a-f9322cec271b.png)
![Screen Shot 2021-03-26 at 11 34 51 PM](https://user-images.githubusercontent.com/5890251/112712325-0f686e80-8e8c-11eb-9cc0-eea17fba4c79.png)

On Step 5, to edit sources.list file, follow this step: 
1. ```sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak``` (make a copy first)
2. ```echo "new line of text" | sudo tee -a /etc/apt/sources.list``` (edit)
3. ```sudo apt-get update```

On Step 7, ran into this error:

![Screen Shot 2021-03-26 at 11 49 26 PM](https://user-images.githubusercontent.com/5890251/112712637-14c6b880-8e8e-11eb-961b-fc95d8a12cb6.png)


Solution: 

1. ```sudo apt-get install openssl```
2. ```sudo apt-get install libssl-dev```
 
 or try this 
 
 https://stackoverflow.com/questions/65520711/problem-installing-nginx-on-ubuntu-20-04-aws-ec2-node

Source: https://www.nginx.com/blog/setting-up-nginx/

(on mac)

![Screen Shot 2021-03-26 at 10 31 17 PM](https://user-images.githubusercontent.com/5890251/112712327-12fbf580-8e8c-11eb-8b46-39f849832f58.png)
![Screen Shot 2021-03-26 at 10 32 05 PM](https://user-images.githubusercontent.com/5890251/112712328-142d2280-8e8c-11eb-96ce-46a23461589b.png)

##### Once ngnix is install
1. ```cd /etc/nginx/``
2. Locate nginx.conf and ```vim nginx.conf``` to edit it. 
3. Add servers, loaderio.txt (to run loader.io test)

![Screen Shot 2021-03-27 at 7 11 23 PM](https://user-images.githubusercontent.com/5890251/112740250-3b3d3000-8f30-11eb-887c-84286d3beeb9.png)

*Note, loaderio-xxx.txt first needs to be added in /home/public directory of your nginx ubuntu instance. Then, you need to reference this location in your nginx.conf file.  

4. Load your nginx instance with relevant paths for example ```http://ec2-3-84-161-79.compute-1.amazonaws.com/reviews/9999``` (make sure to run your aws servers in the background).
5. You are all set! 

#### Useful Commands 
- ```sudo nginx -s reload```
- ```sudo nginx -s stop```
- ```sudo nginx```
- ```sudo vim nginx.conf```



