<p align="center">
  <img src="https://i.postimg.cc/sgQHZgQL/inteco.png" />
</p>
<h3  align="center">Quick Starting Corteza Local Server</h3>
<p align="center">
<a  href="https://devccrm.inteco.at/auth/login">Production</a>
&middot;
<a  href="https://docs.cortezaproject.org/corteza-docs/2023.9/devops-guide/index.html">Corteza Documentation</a>
&middot;
<a  href="https://releases.cortezaproject.org/files/">Releases</a>
</p>



# About
This is a manual, how quickly and easy to run your local Corteza server without waste time. Follow exactly every step that are described bellow. This guide helps to run local environment for development and adding any futures that are necessary in Frontend with followed explanation of deployment to Production.


This is a manual, how quickly and easy to run your local Corteza server without waste time. Follow exactly every step that are described bellow. This guide helps to run  local environment for development and adding any futures that are necessary in Frontend with followed explanation of deployment to Production.


# Setting up the local project

##  Copying a Docker Server locally 
Via ***WinSCP***  connect to the server where "*Corteza_app*" is located. 
Move to the ***root*** folder and then you will finally see Corteza project.
Copy and then paste anywhere on your local Laptop. Now, according to 
[official Corteza guide](https://docs.cortezaproject.org/corteza-docs/2024.9/devops-guide/#deploy-offline), File Structure should look like: 

![file structure](https://i.postimg.cc/HsLwPWmk/1-file-structure.png)



## Configure docker-compose.yaml
Your docker-compose.yaml file supposed to be looked like: 
![docker file](https://i.postimg.cc/Gp2LDNnt/2-docker-file.png)

***Important:***
**`The version may differ from template above.`**

In docker-compose.yaml we will also have to mount more files into ***volumes*** . More about it you could see <a  href="#setting-up-volumes-in-docker-composeyaml">here</a>

You will have to uncomment this line:
```bash
VIRTUAL_HOST: localhost:8080
```

Now it looks like: 
![uncomment the VIRTUAL_HOST](https://i.postimg.cc/kGhZkjh1/3-docker-uncommented.png)

## Configure .env

![.env file](https://i.postimg.cc/6pKR7rHm/4-env-file.png)

***Important:***
**`The version may differ from template above.`**

### 1. Domain
Change **DOMAIN** on the localhost, that you already set up in the ***docker-compose.yaml*** 
 *(by default is expected as 8080)* and should look like: 
![domain](https://i.postimg.cc/XNdSwYTt/5-domain.png)

### 2. Database

Copy of production Data Base should be configured locally via **pgAdmin 4**

***Important:***
**`Make sure that the PostgreSQL version on local laptop versions and PostgreSQL database version are matched.`**

After successful restoration of the database on the local laptop. 
Check, if database is ready to be connected to docker server due to this command on CMD: 
```bash
pg_isready   
```
Successful output will look like that: 
![cmd output](https://i.postimg.cc/zG1Zfbpx/6-cmd-output.png)

We can also connect to Database via **pg_Admin 4** or **CMD** (In this example via CMD) to ensure that all data were fetched correctly.
Enter the command, that is below and connect as *"postgres"* user to database (**database name may be different, check pgAdmin 4**)
![connection to Database](https://i.postimg.cc/7h8MnvBD/7-connection-to-DB.png)

Then, enter database password (by default is: "1234" or "0000") .
And now we can receive, for example, the user "Corteza Service" or any other that exists in the DB.
If the output looks like this, the database data was retrieved successfully:
![data output](https://i.postimg.cc/qq7tVjDS/7-data-output.png)

### Setting up the *DB_DSN* value in .env
Replace underlined values on your local:
 - user (username of database, by default is "postgres");
 - password (database password, by default is "1234" or "0000");
 - database (database name).
  
![ configure DB_DSN ](https://i.postimg.cc/CKB5NRnT/9-configure-connection-to-DB.png)

It might look like this: 
![ example DB_DSN ](https://i.postimg.cc/8CK31CZ5/10-exemple-DB-DSN.png)

### 3. Authorization
As in the <a  href="#1-domain">Domain</a>  settings, **AUTH_BASE_URL** should be changed on the *localhost*, that you set already up in Domain and docker-compose.yaml (VIRTUAL_HOST).

![ example DB_DSN ](https://i.postimg.cc/kgXBVnQ8/11-auth-value.png)

# Running docker server
Now you should install the official [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/) and then inside your `corteza` directory (next to your `docker-compose.yaml` and `.env` files), run the docker compose, due to command: 
```bash	
docker-compose up -d
```
Launch the *localhost:8080* on your browser, and you will see Auth. page: 
![ Authorization page ](https://i.postimg.cc/DzFzL73n/12-auth-screen.png)


**!`To run down the server use the command:`**
```bash
docker-compose down
```
Log in **not with** Domain Credentials, but enter Email and corresponding Password users with admin permission. It might be **it-service** user.
After that you will have access to your local CRM system:
![ main page ](https://i.postimg.cc/XNRhL8Q7/13-crm-main-page.png)

# Corteza-vue
## Bit bucket
Corteza-vue project may look like: 
<p align="center">
<img src="https://i.postimg.cc/52NLRHZ2/14-structure-vue-corteza.png" alt="HTML tutorial" style="width:42px;height:42px;">  
</p>

## Setting up node and updating packages 
Every one directory inside your `corteza-vue project` is **a separate local project** , that are supposed to be run independently  *on different localhosts*, for this reason we have to configure every project (admin, compose, etc.) **alone** but on **the same** way.	

### config.js
Lat's set up for example **Admin** "project", other projects could be set up the same as this one. Inside `admin` directory move on **`public`** folder and create over there  **`config.js`** file.  Add into this file this code: 
```javascript
window.CortezaAPI = 'http://localhost:8080/api'
```
**!`To run down the server use the command:`**
### node
### building 
## Setting up volumes in docker-compose.yaml

## sample of running local server

# Links 


Despite the fact that 


