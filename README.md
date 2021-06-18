# Docker Swarm - Nginxproxymanager with Letsencrypt and Duckdns 

## DUCKDNS
Duckdns is a free service that will point a DNS (sub domains of duckdns.org) to an IP of your choice. The service is completely free and does not require reactivation or forum posts to maintain its existence. Create an account to use the service and after that create a subdomain.

## NAT Ports
- Need to create route/nat for ports on your modem.

    | Port                   | Desc
    | -------------------    | ----------------------------------------------------------------
    | 80                     | Port HTTP - Needed for letsencrypt dnschallenge 
    | 443                    | Port HTTPS

## Install Project

- Install git - Centos7 
    ```bash
    $ dnf install -y git
    ```

- Install git - debian/ubuntu 
    ```bash
    $ apt install -y git
    ```

- Cloning repository 
    ```bash
    git clone https://github.com/bernardolankheet/nginxproxymanager-duckdns.git && cd nginxproxymanager-duckdns  
    ```

- Starting Docker Swarm
    ```bash
    $ docker stack init
    ```

- Change env_file  
    
    Change variables within env_files

    | Environment            | Desc
    | -------------------    | -------------------------------------------------------------------------------------
    | .env_db                | Environment to MariaDB
    | .env_duckdns           | Environment to Duckdns domains
    | .env_npm               | Environment to Nginx proxy Manager

- Change the variables according to your Domains **.env_duckdns**

    | Environment            | Desc
    | -------------------    | -------------------------------------------------------------------------------------
    | SUBDOMAINS             | Subdomain created in duckdns, do not put the complete domain (test.duckdns.org)
    | TOKEN                  | duckdns API access token for external IP update

- Default configs

    | Environment            | Default
    | -------------------    | -----------------------
    | TZ                     | America/Sao_Paulo

## Letsencrypt

- Letsencrypt has a limitation of certificate requests per week (be careful not to make too many requests, if I'm not mistaken it is 7 per week, and it is blocked), for testing, it is recommended to use the staging api, and when everything is OK, migrate for the production environment they provide, traefik by default uses the ACME of the production environment, so there is a parameter that we can define the test api (acme.caserver). In this file I left it commented, in the first moments I recommend using the test api, just remove the comment (#), when accessing the url you will see an untrusted certificate warning, but checking the certificate, you will see that it was issued by CN "Fake LE Intermediatre X1" and "Fake LE Root X1", this means that your certificate was issued by Letsencrypt, but this CN is not listed as trustworthy, since it is a test.

- Letsencrypt certificates last 90 days, nginxproxymanager automatically renews

## Starting services

- Starting services
    ```bash
    $ docker stack deploy --prune -c docker-compose.yml nginxpmanagerduckdns 
    ```  

- Viewing services and stack
    ```bash
   $ docker stack ls
   $ docker service ls
    ```

- Viewing containers
    ```bash
    $ docker container ls
    ```

- Viewing Logs
    ```bash
    $ docker container logs namecontainer/ID
    $ docker service ps --no-trunc nginxpmanagerduckdns
    ```

## Access with web browser
- Access your duckdns subdomain url via browser 
    1) http://domain.duckdns.org:81        ---- Nginx Proxy Manager 
	2) http://domain.duckdns.org        ---- Nginx default page

- Default Admin User:
    ```bash
	Email:    admin@example.com
	Password: changeme
    ```	   

## Removing/updating stack
- To remove the stack, just use
    ```bash
    $ docker stack rm nginxpmanagerduckdns
    ```

- Make some change to yml, and need to update the service, just run the script again
    ```bash
    $ docker stack deploy --prune -c docker-compose.yml nginxpmanagerduckdns
     ```

## Project WIKI [Nginxproxymanager](https://nginxproxymanager.com/guide/#project-goal)
## [DUCKDNS](https://www.duckdns.org)
