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
in frontend that loadbalancer resides and backend that php container and the slave nginx containers resides.Through nginx.conf file for loadbalancer we create an upstream pool with backend servers in order loadbalancer to communicate with them and with the use of proxy_pass http://backend; directibe to route incoming HTTPS requests to backend pool.
Also this project can combine with HashiCorp Vault in ordert to secure user
credentials.Furthermore a sql database with replication to persist the login credentials
and any other customer profile settings,redis for session persistans and many more.
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
  - `html`: mounts index.php to /var/www/html inside the **master** nginix server.
  - `./nginx.conf`: mounts the localy create nginx.conf file to /etc/nginx/conf.d/default.conf inside the **master** nginix server.
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

### **3.nnginx web server** (`web_server2`)

This service runs a **nginx** server inside a Docker container.

- **Image**: Uses the official nginx image.
- **Container Name**: `slave2`
- **Network Mode**: Uses the **bridge network** (directly binds to the host’s network stack) but runs inside the `backend` network.
- **depends_on**: Always start first the **php** server without it `web_server2` cannot start and function to serve content.
- **Bind Mounts**:
  - `html`: mounts index.php to /var/www/html inside the **master** nginix server.
  - `./nginx.conf`: mounts the localy create nginx.conf file to /etc/nginx/conf.d/default.conf inside the **slave** nginix server.

---

## **Networks**
- `frontend`: A bridge network for communication between the `postgres-db` and `python-app`.
- `backend`: A bridge network for communication between the `load_balancer` with  `web_server1`
`web_server2` and `php`.
---

## **Volumes**
- `my_volume`: Stores PostgreSQL data persistently to prevent data loss when the container restarts.

---

## wait-for-it

This script is designed to wait for a PostgreSQL server to become available before executing a command. It's commonly used in Docker container setups, where the Python application or other services may depend on a PostgreSQL database being up and ready.

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
   
   ```

2. Create a `.env` file for environment variables (database credentials) if not using Vault yet.

3. Build and start the Docker containers:
   ```bash
   docker-compose up --build
   ```

4. Open the Python app container and interact with the calculator.

### Executing program

Run the application using:
```bash
docker-compose up
```

Once the app is running, run in other terminal:
```bash
docker container ls
```

connect to python app to run the python script and the same for the other containers:
```bash
docker exec -t <container> /bin/bash
```

to connect to to postgresql run the following:
```bash

```

List available databases:
```bash

```

Switch connection to a new database:
```bash

```

List available tables:
```bash

```

Describe a table such as a column, type, modifiers of columns, etc.:
```bash

```

## Help

For common issues, check the logs or run the following command for help:
```bash

```

## Authors

Contributors names and contact info

ex. Vasilhs S.  


## Version History



## License


## Acknowledgments


