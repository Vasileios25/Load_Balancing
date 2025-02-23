# Load_Balancing

The project is a simple load balancing  application running inside a Dockerized environment with four containers:
1. **Nginx Load Balancer**: Handles user trafic and performs basic load balancing operations.
2. **web_server1**: Serves simple php form for user credentials.
3. **web_server2**: Serves simple php form for user credentials.
4. **php**: Simple html user credentials form .

## Description

This project ilustrates the load balancing concept with docker containers in order
to offer a simple php user credentials form to the customers, it can used for 
bigger projects for example a web site for orders, use health checks at load balancing level
in order to minimize the delay and increase the customer sattisfaction. Separate the brindge network
in frontend that loadbalancer resides and backend that php container and the slave nginx containers resides.Through nginx.conf file for loadbalancer we create an upstream pool with backend servers in order loadbalancer to communicate with them and with the use of proxy_pass http://backend; directive to route incoming HTTPS requests to backend pool.
Also this project can combine with HashiCorp Vault in ordert to secure user
credentials.Furthermore a sql database with replication to persist the login credentials
and any other customer profile settings,redis for session persistence and many more. Let’s Encrypt SSL Certifications enbaled with certbox.
### Features:
- Creation of 4 containers.
- Loadbalancing between 2 nginx servers.
- Serving of a php html user credentials file.


---

## Docker Compose File Explanation

This Docker Compose (`docker-compose.yml`) file sets up a multi-container environment with:
1. **Nginx Load Balancer** (`load_balancer`)
2. **nginx web server** (`web_server1`) for secrets management
3. **nginx web server** (`web_server2`), which interacts with the database
4. **php container** (`php`), which interacts with the database

---

## **Services**

### **1. Nginx Load Balancer** (`load_balancer`)

This service runs a **nginx** server inside a Docker container.

- **Image**: Uses the official nginx image.
- **Container Name**: `master`
- **Exposed Ports**: Internally exposes port **80** (default http port).
- **Bind Mounts**:
  - `./nginx.conf`: mounts the localy create nginx.conf file to /etc/nginx/conf.d/default.conf inside the **master** nginix server.
  - `./certs_nginx/fullchain.pem`: mounts the fullchain.pem to /etc/letsencrypt/ dir inside the container
  - `./certs_nginx/privkey.pem`: mounts the privkey.pem to /etc/letsencrypt/ dir inside the container
- **depends_on**: Always start first the slave nginx servers without them loadbalancer cannot start and function.
- **Network**: Connects to both `frontend` and `backend`for communication with other services.

---

### **2.nnginx web server** (`web_server1`)

This service runs a **nginx** server inside a Docker container.

- **Image**: Uses the official nginx image.
- **Container Name**: `slave1`
- **Network Mode**: Uses the **bridge network** (directly binds to the host’s network stack) but runs inside the `backend` network.
- **depends_on**: Always start first the **php** server without it `web_server1` cannot start and function to serve content.
- **Bind Mounts**:
  - `html`: mounts index.php to /var/www/html inside the **master** nginix server.
  - `./nginx.conf`: mounts the localy create nginx.conf file to /etc/nginx/conf.d/default.conf inside the **slave** nginix server.

---

### **3.nginx web server** (`web_server2`)

This service runs a **nginx** server inside a Docker container.

- **Image**: Uses the official nginx image.
- **Container Name**: `slave2`
- **Network Mode**: Uses the **bridge network** (directly binds to the host’s network stack) but runs inside the `backend` network.
- **depends_on**: Always start first the **php** server without it `web_server2` cannot start and function to serve content.
- **Bind Mounts**:
  - `html`: mounts index.php to /var/www/html inside the **master** nginix server.
  - `./nginx.conf`: mounts the localy create nginx.conf file to /etc/nginx/conf.d/default.conf inside the **slave** nginix server.

---

### **4.php container** (`php`)

This service runs a **php** server inside a Docker container.

- **Image**: Uses the official php:7.4-fpm image.
- **Container Name**: `php`
- **Network Mode**: Uses the **bridge network** (directly binds to the host’s network stack) but runs inside the `backend` network.
- **Bind Mounts**:
  - `html`: mounts index.php to /var/www/html inside the **master** nginix server.
  - `./nginx.conf`: mounts the localy create nginx.conf file to /etc/nginx/conf.d/default.conf inside the **slave** nginix server.


## **Networks**
- `frontend`: A bridge network for communication between the `postgres-db` and `python-app`.
- `backend`: A bridge network for communication between the `load_balancer` with  `web_server1`
`web_server2` and `php`.

---

## LoabBalancer and backend nginx.conf 
- First we declare the `proxy_cache_pat` directory for caching purposes.
- `/var/cache/nginx` is the place where the actual cache stored.
- `levels=1:2` sets the number of subdirectory levels in cache.
- `keys_zone=cache:10m` defining shared memory zone named `Cache` with maximum size `10 MB`. It holds all active keys and metadata of the cache. So, whenever nginx checks if a page was cached, it consults the shared memory  zone first, then seek the location of actual cache in `/var/cache/nginx` if cache exist.
- `max_size` maximum size of cache e.g. files size on `/var/cache/nginx`.
- `inactive=1h` specify maximum inactive time cache can be stored. Cached data that are not accessed during the time specified by the inactive parameter get removed from the cache regardless of their freshness.
- After 10 minutes (from `proxy_cache_valid`), the cached item becomes stale, but it remains in the cache as long as it is accessed within the inactive window (e.g., 1 hour).
If it's not accessed for 1 hour (exceeding the inactive time), it will be removed from the cache, and the next request will get the content from the backend.
- `upstream` directory is like a pool or cluster of servers that can be reference by `proxy_pass`.
- 2 `server` directoried used in order if http traffic hit the server will redirected to server directory tht will serve https content.
- `FastCGI` protocol used that based on the earlier CGI. Improves PHP processing by not open a new process for any new request. `fastcgi_pass`  define the actual server to proxy to using the FastCGI protocol,
in this implementation the `php` container with `9000 port`. Because we changing protocols some parameters should put here in order to work like `SCRIPT_FILENAME` tells FastCGI (like PHP-FPM) the full path of the script that needs to be executed. It's used to ensure that the FastCGI server processes the correct file when a request is made to a PHP or other FastCGI-based script.


---

### **1.Assign Variables:**
```bash

```

### **1.Assign Variables:**


## Getting Started

### Dependencies

* 
* Docker
* Docker Compose

### Installing

1. Clone the repository:
   ```bash
   git clone https://your-repo-url.git
   cd Load_Balancing
   ```

2. Build and start the Docker containers:
   ```bash
   docker-compose up --build
   ```



### Executing program

Run the application using:
```bash
docker-compose up
```

Once the app is running, run in other terminal:
```bash
docker container ls
```

connect to any app:
```bash
docker exec -t <container> /bin/bash
```

## Help

For common issues, check the logs or run the following command for help:
```bash
docker-compose logs
```
For security reasons certs have nor uploaded.
Will auto renew
## Authors

Contributors names and contact info

ex. Vasilhs S.  


## Version History



## License


## Acknowledgments


