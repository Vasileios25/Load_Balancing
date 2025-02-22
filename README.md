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
in order to minimize the delay and increase the customer sattisfaction
Also this project can combine with hashicorp vault in ordert to secure user
credentials.Furthermore a sql database with replication to persist the login credentials
and any other customer profile settings,redis for session persistans and many more.

### Features:
- 
- .
- .
- .

---

## **1. Initial Setup**

- **Imported Libraries**:
  - ``: .
  - ``: .
  - ``: .
  
- .
- .

---

## **2. Connecting to PostgreSQL Database**

- (`my_db`) .
- .

---

## **3. Retrieving Previous Data from Database**

- `Calculator` :
  ```;
  ```
- If no previous data exists, it initializes the statistics to `[0, 0, 0, 0, 0, 0]` (representing counts for each operation).

---

## **4. User Interaction and Calculator Operations**

:
```

```

- :
  - Addition (1): `first_number + second_number`
  - Subtraction (2): `first_number - second_number`
  - Multiplication (3): `first_number * second_number`
  - Division (4): `first_number / second_number` (with handling for division by zero)
  - Square Power (5): `math.pow(number, 2)`

---

## **5. Handling Errors**

The script includes exception handling for:
- Invalid input: If the user enters a non-numeric value, it prompts them to try again.
- Zero division: If division by zero is attempted, it prints an error message and sets the result to 0.

---

## **6. Updating Statistics**

After the user exits (choosing option 6), the program:
- Increments the entry ID (`id += 1`).
- Prints a summary of how many times each operation was used.

---

## **7. Writing Data to CSV**

The script creates a `Statistics.csv` file and writes the updated statistics:
```python
with open('Statistics.csv', 'w', newline='') as csvfile:
    my_writer = csv.writer(csvfile, delimiter=',')
    my_writer.writerow(input_variable)
```
- Reads the CSV data back and stores it in a list (`all_value`).

---

## **8. Storing Data in PostgreSQL**

The script inserts the updated statistics into the `Calculator` table using the SQL query:
```sql
INSERT INTO Calculator VALUES (%s, %s, %s, %s, %s, %s);
```
- Uses `executemany()` to insert multiple rows if needed.

---

## **9. Closing the Database Connection**

- Commits the changes and closes the database connection.
- Prints "Thanks" to indicate that the program has successfully completed execution.

---

## Docker Compose File Explanation

This Docker Compose (`docker-compose.yml`) file sets up a multi-container environment with:
1. **PostgreSQL Database** (`postgres-db`)
2. **Vault Service** (`vault`) for secrets management
3. **Python Application** (`python-app`), which interacts with the database

---

## **Services**

### **1. PostgreSQL Database (`postgres-db`)**

This service runs a **PostgreSQL 13** database inside a Docker container.

- **Image**: Uses the official PostgreSQL 13 image.
- **Container Name**: `postgres-container`
- **Exposed Ports**: Internally exposes port **5432** (default PostgreSQL port).
- **Environment Variables**: Loads database credentials from `.env` variables.
- **Volumes**:
  - `init_db.sql`: Runs an SQL script at startup to create the **Calculator** table:
    ```sql
    CREATE TABLE IF NOT EXISTS Calculator (
      id integer,
      addition integer,
      substraction integer,
      multiplication integer,
      division integer,
      square integer
    );
    ```
  - `my_volume`: Stores database data persistently.
- **Restart Policy**: Always restarts the container if it stops.
- **Network**: Connects to `my-network` for communication with other services.

---

### **2. Vault (`vault`)**

This service runs **HashiCorp Vault**, used for securely storing and managing secrets.

- **Build Context**: Uses a custom `Dockerfile.vault` to build the container.
- **Container Name**: `vault`
- **Network Mode**: Uses the **host network** (directly binds to the host’s network stack).
- **Environment Variables**:
  - `VAULT_ADDR`: Specifies the Vault server address.
  - `VAULT_TOKEN`: Uses an environment variable for authentication.
- **Volumes**:
  - Mounts the Vault configuration file (`vault-config.hcl`).
  - Mounts a `.env` file to store passwords.
  - `apply-policies.sh`: Script for applying security policies.
  - *(Optional)* `kv-policy.hcl`: Can be added for Vault policy configuration.

---

### **3. Python Application (`python-app`)**

This service runs a **Python script** that connects to the database and performs calculations.

- **Restart Policy**: .
- **Build Context**: .
- **Container Name**: ``
- **Dependencies**:  `` .
- **Command & Entry Point**:
  - `` ``.
  - Executes the Python script (`Calculator.py`) after confirming PostgreSQL is available.
- **Network**: Connects to `my-network` for database communication.

---

## **Networks**
- `my-network`: A bridge network for communication between the `postgres-db` and `python-app`.

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


