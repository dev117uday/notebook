# How to Deploy React/Vue/Angular on a VM

![vue on vm](assets/vue_on_vm.jpg)

_kubernetes would be too overkill_

In March, i tried learning nginx, a load balancer, reverse proxy and much more. At the same time, i was looking into buying a domain for my portfolio site. Now trying to apply this new super power that i got, i tried to deploy it my static resume site on a virtual machine. How i did it, continue reading the article ...

### Step 1 : Dockerize your site

I use vite-vue to make my portfolio site. Why you may ask, because i can break down different sections into components and also easier for me to update information. the final build is compact and no one can copy it easily (i am not a celebrity nor a distinguished software engineer).

This is my config file : `Dockerfile`. This would work with react, vue, angular or any framework (hope it works for others)

```dockerfile
FROM node:15.12.0-alpine3.10 as build-stage
WORKDIR /app
COPY . ./
RUN yarn install && yarn run build

FROM nginx as production-stage
RUN mkdir /app
COPY --from=build-stage /app/dist /app
COPY nginx.conf /etc/nginx/nginx.conf
```

- If you are solving this article, please the check the version of docker base image [here](https://hub.docker.com/_/node). 
- Also look at the final build folder your framework generates, for vue its `/dist`, else nothing will work


Create another file named `.dockerignore`

```
**/node_modules
**/dist
```

again look at `/dist`

Create a file named : `nginx.conf` file in **your project directory**, not is your VM's `/etc/nginx`
```
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
  worker_connections  1024;
}
http {
  include       /etc/nginx/mime.types;
  default_type  application/octet-stream;
  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
  access_log  /var/log/nginx/access.log  main;
  sendfile        on;
  keepalive_timeout  65;
  server {
    listen       80;
    server_name  localhost;
    location / {
      root   /app;
      index  index.html;
      try_files $uri $uri/ /index.html;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
      root   /usr/share/nginx/html;
    }
  }
}
```

### Create a virtual Machine

Now this is the part where i suppose you have a VM and have a bit of knowledge about it
- If you are using AWS EC2, then create a new security group giving access to everyone on port 80
- If you are using Digital Ocean or similar service, then i hope it is done for you 


After creating a VM, install the following 

```sh
sudo apt-get install nginx nano letsencrypt
```
- `nano` for editor, you may also need git.

Then run : `nginx -s reload` and goto the external IP of your VM using browser, you will see nginx's welcome page, if you dont, try restarting the VM or wait for 5 mins else there is some networking problem.

All done till here, great

Then install docker

```sh
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

### Building docker container

- Clone the repo in your VM
- `cd` into it
- Run

```
sudo docker build . -t resume
sudo docker run -d -p 8080:80 resume
```

### Making it online

If you dont have your own domain name yet, keep reading else skip to next part

- As admin, delete the default `nginx.conf` file in the `/etc/nginx` folder 
```
sudo rm -f /etc/nginx/nginx.conf
```
- Create a new `nginx.conf` in the `/etc/nginx` directory by running

```
sudo nano /etc/nginx/nginx.conf
```

- You have to a super user in order to use this file
- Paste the following content

```
http {
  server {
    listen 80;
    location / {
      proxy_pass http://127.0.0.1:8080/;
    }
  }
}

events { }
```

Save your file, by pressing `ctrl+o`, confirming it and exit by pressing `ctrl+x`

Then run 
```sh
sudo nginx -s reload
```

You goto the external IP of your VM, you will see your site 

#### If you have your own domain

Run the following to generate temp certificate

```sh
sudo certbot certonly --standalone
```

Go through all the steps

**Keys are located at :** 

```
/etc/letsencrypt/live/[ dns ]/fullchain.pem
/etc/letsencrypt/live/[ dns ]/privkey.pem

```

open the `nginx.conf` in `/etc/nginx` folder and replace the content with `{dns without http and www}`

```
http {
    server {
        listen 80;
        listen 443 ssl http2;
        ssl_certificate /etc/letsencrypt/live/{dns without http and www}/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/{dns without http and www}/privkey.pem;
        ssl_protocols TLSv1.3;
        location / {
          proxy_pass http://127.0.0.1:8080/;
        }
    }

}

events { }
```

Run `sudo nginx -s reload` and wait and check the domain

Hope you like, if you find any problem, error, please comment below