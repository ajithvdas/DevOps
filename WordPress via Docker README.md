# Documentation: Deploying WordPress via Docker
This guide outlines the process of deploying a WordPress site using a MySQL backend via Docker containers.

## Prerequisites

OS: Ubuntu 22.04

Docker Engine installed and running.

Sufficient disk space (verified via ```df -h```).

Sudo privileges on the host machine.

## Deploy the MySQL Database
First, we pull and run the MySQL image. This container will store all the WordPress data.

```
sudo docker run --name wordpressmysql -e MYSQL_ROOT_PASSWORD=password -d mysql
```

<img width="864" height="203" alt="image" src="https://github.com/user-attachments/assets/701f043e-b41c-492a-ad60-b293c091c3f5" />


## Parameter Breakdown:

```--name wordpressmysql:``` Assigns a specific name to the container for easier management.

```-e MYSQL_ROOT_PASSWORD=password:``` Sets the mandatory root password for the database.

```-d:``` Runs the container in detached mode (background).

```mysql:``` The official image pulled from Docker Hub.


## Deploy the WordPress Container

Next, we run the WordPress container and link it to the MySQL container created in the previous step.

```sudo docker run --name mywordpress --link wordpressmysql:mysql -P -d wordpress```

<img width="899" height="417" alt="image" src="https://github.com/user-attachments/assets/53459545-3256-4de6-82b0-d7ee3d6695d3" />


## Parameter Breakdown:

```--link wordpressmysql:mysql:``` Securely connects the WordPress container to the MySQL container.

```-P:``` Publish all exposed ports. This maps the container's internal port 80 to a random high-number port on your host machine.


## Verify and Access the Application

### Step A: Check Container Status

Verify that both containers are running properly:

```sudo docker ps```

<img width="1024" height="109" alt="image" src="https://github.com/user-attachments/assets/9eef3d37-983a-495b-98cb-14926c90e116" />

### Step B: Identify the Assigned Port

Since we used the ```-P``` flag, Docker assigned a random port. Look at the PORTS column in the output of the previous command. It will look like this: ```0.0.0.0:32768->80/tcp```

### Step C: Access via Web Browser

Find your Host IP address: ```hostname -I```

Combine the IP and the Port found in Step B.

URL Format: ```http://<Host-IP>:<Assigned-Port>``` Example: ```http://172.30.158.67:32768```

<img width="863" height="665" alt="image" src="https://github.com/user-attachments/assets/812c95f0-79aa-45a6-b9a7-73af99cb3c23" />
