# DOCKER COMPOSE WITH POSTGRESQL DRIVER

### Step 1: 
 Copy current [docker compose](./docker-compose.yml) and postgresql driver files in the same directory.  

 ![alt text](image.png)


## Step 2:
-  Verify docker-compose.yml and check if you need to change any port.   If there are conflict please change.
-  Consider changing your username or password..  


```yml
version: '3'

services:
  nifi:
    cap_add:
      - NET_ADMIN # low port bindings
    image: apache/nifi:2.0.0
    environment:
      - TZ=America/Montreal    # By default container with Montreal tyme zone (change if neccesary. )
      - NIFI_WEB_HTTP_HOST=nifi
      - SINGLE_USER_CREDENTIALS_USERNAME=nifi-12345   # You can change for desired user
      - SINGLE_USER_CREDENTIALS_PASSWORD=nifi-12345   # You can change for desired password
      
    container_name: nifi
    ports:
    # Check if you need to changes the port before the colon ":"
      - "58081:8080/tcp" # HTTP interface.   
      - "8443:8443/tcp" # HTTPS interface
      - "514:514/tcp" # Syslog
      - "514:514/udp" # Syslog
      - "2055:2055/udp" # NetFlow
    volumes:
    # Check the persistent volumes names are not equal to any docker previous installation
      - nifi-drivers:/opt/nifi/nifi-current/drivers   
      - nifi-certs:/opt/certs
      - nifi-conf:/opt/nifi/nifi-current/conf
    # Copy postgresql driver in container.     
      - ./postgresql-42.7.4.jar:/opt/nifi/nifi-current/drivers/postgresql-42.7.4.jar
      
    restart: unless-stopped

volumes:
  nifi-drivers:
    name: nifi-drivers
  nifi-certs:
    name: nifi-certs
  nifi-conf:
    name: nifi-conf
```


## Step 3:
-  Open command line (terminal) in the current directory of [docker compose](./docker-compose.yml) and postgresql driver files.

## Step 4:
-  To execute this command, ensure Docker is installed beforehand.

```powershell
docker compose up -d
```

or alternative

```powershell
docker-compose up -d
```

## Step 5:
-  Open nifi in the browser,  change the port if you has prevously modify this [docker compose](./docker-compose.yml) file.

