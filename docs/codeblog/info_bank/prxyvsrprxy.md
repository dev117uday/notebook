# Proxy vs Reverse Proxy

Understanding it easily, once and for all.

So you might have come across these 2 terms, proxy and reverse proxy while browsing the internet or going through system settings. But do you really understand the differences … ? This article will settle the difference between proxy and reverse proxy.


## Proxy
Let's say you want to visit a website ( something.com ) on the internet which is blocked by your ISP or your company or the country you live in.So instead of going through to the site directly, you go to a proxy computer which then goes to `www.something.com` for you and returns the content from there.

**According to the wikipedia definition**
_In computer networking, a proxy server is a server application or appliance that acts as an intermediary for requests from clients seeking resources from servers that provide those resources. A proxy server thus functions on behalf of the client when requesting service, potentially masking the true origin of the request to the resource server._

![proxy](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4c4eqlpyykhvnfl48v7s.png)

Now you may ask :
- Aren’t the proxy servers also banned from visiting `www.somthing.com` ?
- Or you might be blocked from visiting the proxy server ?

Well to answer the latter, proxy servers are usually hosted in the cloud and have dynamic IP or they are just normal computers that aren't blocked. Coming to the first question, proxy servers are usually hosted somewhere where there is no access restriction and can access any site of the internet. Again if you host the proxy server on the same network as you are in or in the same country, then yes, the proxy server will also be blocked from accessing `www.something.com`.

Proxy Servers can be used for 
- Monitoring and Filtering 
- Content Control Software
- Filtering encrypted Data
- Bypassing filters and censorship
- Improving Performance ( as a cache server ) 
- Logging events and much more ….

## Reverse Proxy
Reverse Proxy is something that doesn't show in your system settings or browsing the web but it's something that every developer should know. Let's take an example where you host a website on `www.something.com` and it became popular. The number of people reaching your website increases dramatically and as new users start coming, your web server starts to slow down and starts dropping connection. Now what do you do ? 

You decided to upgrade the computer on which it is hosted, well that does solve the problem, but to what extent. Eventually if the traffic increases your server won't be able to handle the users.

Then let's say you decided that you will add another server to handle the traffic. But how will you go out and tell everyone and deviate  traffic. How will the user know which server to hit to get access to the website? This is where a reverse proxy comes in.

Instead of choosing between which server to go to, the user will hit your reverse proxy and the reverse proxy will deviate some users to `webserver1` and some users to `webserver2`. And if you add more servers, you can just tell the address of the newly added servers to reverse proxy. This concept is known as load balancing.

![reverse proxy](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/l4repqwr3ha7wkt6rnfs.png)

You may ask, what if the reverse proxy gets overwhelmed, well yes that can happen and does happen, but then we have other methods to handle such massive traffic using DNS based load balancing or breaking your service into smaller parts and hosting them differently. 

**Uses of reverse proxy**
- Load balancing – A reverse proxy server can act as a “traffic cop,” sitting in front of your backend servers and distributing client requests across a group of servers in a manner that maximizes speed and capacity utilization while ensuring no one server is overloaded, which can degrade performance. If a server goes down, the load balancer redirects traffic to the remaining online servers.
- Web acceleration – Reverse proxies can compress inbound and outbound data, as well as cache commonly requested content, both of which speed up the flow of traffic between clients and servers. They can also perform additional tasks such as SSL encryption to take load off of your web servers, thereby boosting their performance.
- Security and anonymity – By intercepting requests headed for your backend servers, a reverse proxy server protects their identities and acts as an additional defense against security attacks. It also ensures that multiple servers can be accessed from a single record locator or URL regardless of the structure of your local area network.


## Difference between proxy and reverse proxy
Though reverse proxy and proxy do the exact same thing, masking the true origin of the user, there is a slight difference in the way the operate

Unlike a traditional proxy server, which is used to protect clients, a reverse proxy is used to protect servers. A traditional forward proxy server allows multiple clients to route traffic to an external network. For instance, a business may have a proxy that routes and filters employee traffic to the public Internet. A reverse proxy, on the other hand, routes traffic on behalf of multiple servers. 
A reverse proxy effectively serves as a gateway between clients, users, and application servers. It handles all the access policy management and traffic routing, and it protects the identity of the server that actually processes the request.





