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

Source: https://www.nginx.com/blog/setting-up-nginx/

(on mac)

![Screen Shot 2021-03-26 at 10 31 17 PM](https://user-images.githubusercontent.com/5890251/112712327-12fbf580-8e8c-11eb-8b46-39f849832f58.png)
![Screen Shot 2021-03-26 at 10 32 05 PM](https://user-images.githubusercontent.com/5890251/112712328-142d2280-8e8c-11eb-96ce-46a23461589b.png)

#### Useful Commands 
- ```nginx -s reload```
- ```nginx -s stop```
- ```nginx```

