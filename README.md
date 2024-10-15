# Redis Configuration with AWS EC2

## Installation
**1. Launch Ubuntu AWS EC2 instance**.

**2. Update your local apt package cache:**
```bash
sudo apt update
sudo apt upgrade
```
**3. Install Redis:**
```bash
sudo apt install redis-server
```
**4. Initialize system to manage Redis as a service:**
```
sudo vim /etc/redis/redis.conf   # open this file
```
* Find **supervised** keywork and change the configuration
```
supervised no
```
* Change to
```
supervised systemd
```
**5. After that, connect with Redis server by the command:**
```bash
redis-cli
```
**6. Create a User in Redis and access redis-cli with the user:**
* First open the **redis.config** file by the below command:
```bash
sudo vim /etc/redis/redis.conf
```
* Now the config file scroll down the Security section and look for the commented directive:
```bash
# requirepass foobared
# Uncomment this cmd line with your secured password
Example: requirePass securePassword
```
* Create a new user of your own go inside redis-cli and run the below command:
```bash
ACL setuser username allcommands allkeys on >password
```
Change username and password with your credentials

**7. Expose Redis to the public ip:**
To expose the Redis server to the public IP address just go to the config file
* Locate this line
```bash
bind 127.0.0.1 ::1
```
* And change it to below line:
```bash
bind 0.0.0.0
```
**8. Create Redis server replica**
```bash
replicaof hostIp hostPort
```
**9. Choosing the eviction policy**
* Redis will select what to remove when maxmemory is reached
```bash
 maxmemory-policy allkeys-lru
``` 
**10. Using I/O threads it is possible to easily speedup two times Redis without resorting to pipelining nor sharding of the instance.**
* Enable redis threading
```bash
io-threads 4
```
**11. After saving this restart the redis server by the command:**
```bash
sudo systemctl restart redis
```
**12. enable Redis on system boot:**
```bash
sudo systemctl enable redis-server.service
```
**13. Test the Redis server is working or not:**
```bash
sudo systemctl status redis
```
**14. Redis benchmark-tool to measure the performance of the server**
```bash
redis-benchmark -c 100 -n 1000000 -d 64
```
**-h**: Redis server host;
**-p**: Redis server port;
**-c**: Parallel running connection;
**-n**: No of request

**15. Create client connection**
```bash
url: 'redis://USER_NAME:PASSWWORD@HOST:PORT'
```
