# Apache-Hadoop-Hive

## 1. Setup
Navigate inside the Hive directory on your local and run the single docker compose command to create and start all services required by our Hive cluster:

```
# Initialize all your docker services defined in the docker-compose.yml file
docker-compose up -d
```

Run the command docker stats in another terminal. As docker begins to spin up the containers, their CPU and Memory utilization will stabilize after a few minutes in absence of any other activity on the cluster:

```
docker stats
```

## 2. Using hive
Its time for a quick demo! Log onto the Hive-server and create a sample database and hive table by executing the below command in a new terminal:

```
# Execute a bash terminal inside hive-server container
docker exec -it hive-server /bin/bash
```

Navigate to the employee directory on the hive-server container:
```
cd ..

# Navigate to employee directory
cd employee/
```

Execute the employee_table.hql to create a new external hive table employee under a new database testdb:
```
hive -f employee_table.hql
```

### Validate the setup

On the hive-server, launch hive to verify the contents of the employee table:
```
# Run the hive service
hive

hive> show databases;

# Change your current DB

hive> use testdb;

# Select the employee table
hive> select * from employee;
```

## 3. Validate the container state
It is critical to verify that the Hive tables are maintained between subsequent docker runs and we do not end up losing our progress if docker containers are stopped. Therefore, restart the docker containers and verify that the hive data is persisted or not.

In a new terminal, execute below command to stop all docker containers:

```
# Stop the container services
docker-compose down
```

Once all containers are stopped, run docker-compose up one more time:

```
docker-compose up -d
```

Wait for a few minutes as suggested in step 4 for the containers to come back online, then log onto the hive-server and run the select query:
```
docker exec -it hive-server /bin/bash

hive

# Show the data of the employee table
hive> select * from testdb.employee;
```

Hurray! We still have our data!