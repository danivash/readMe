<p align="center">
  <img src="https://i.postimg.cc/FKfmF4fQ/inteco.png" />
</p>
<h3  align="center">A quick guide to running a local Corteza server</h3>
<p align="center">
<a  href="https://devccrm.inteco.at/auth/login">Production</a>
&middot;
<a  href="https://docs.cortezaproject.org/corteza-docs/2023.9/devops-guide/index.html">Corteza Documentation</a>
&middot;
<a  href="https://releases.cortezaproject.org/files/">Releases</a>
</p>



# About

This is a manual on how to quickly and easily run your local Corteza server without wasting time. Follow each step described below exactly as written. This guide helps you set up a local environment for development and add any features necessary for the front-end, along with an explanation of how to deploy to production.


# Setting up the local project

##  Copying a Docker Server locally 

Via ***WinSCP***  connect to the server where "*Corteza_app*" is located. 
Move to the ***root*** folder and then you will finally see Corteza project.
Copy and then paste anywhere on your local Laptop. Now, according to 
[official Corteza guide](https://docs.cortezaproject.org/corteza-docs/2024.9/devops-guide/#deploy-offline), File Structure should look like: 

![file structure](https://i.postimg.cc/GpswwxNL/1-file-structure.png)



## Configure docker-compose.yaml

Your docker-compose.yaml file is supposed to be looked like: 

![docker file](https://i.postimg.cc/sfJFrXnh/2-docker-file.png)

***Important:***
**`The version may differ from template above.`**

### Mounting the volumes

In docker-compose.yaml we also have to mount more files into ***volumes*** . 

***Recommendation:***
***`Come to this step after`<a  href="#building">`building`</a>`the project`***
You have to mount all dependencies from **`dist`** folders of all projects (admin, compose, etc.). 
Sample for admin index.html:
```bash
"path-to-corteza-vue/admin/dist/index.html:/corteza/webapp/admin/index.html"
```
For all folders it may look like:

![mounted dependencies](https://i.postimg.cc/9MFMYS08/17-docker-compose-should-be-3.png)

### VIRTUAL_HOST

You will have to uncomment this line:
```bash
VIRTUAL_HOST: localhost:8080
```

Now it looks like:

![uncomment the VIRTUAL_HOST](https://i.postimg.cc/fRJ1mvMc/3-docker-uncomment.png)

## Configure .env

![.env file](https://i.postimg.cc/HLMKPscj/4-env-file.png)

***Important:***
**`The version may differ from template above.`**

### 1. Domain

Change **DOMAIN** on the localhost, that you already set up in the ***docker-compose.yaml*** 
 *(by default is expected as 8080)* and should look like: 
 
![domain](https://i.postimg.cc/RFQY4sTB/5-domain.png)

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

![cmd output](https://i.postimg.cc/rsfYtDbZ/6-cmd-output.png)

We can also connect to Database via **pg_Admin 4** or **CMD** (In this example via CMD) to ensure that all data were fetched correctly.
Enter the command, that is below and connect as *"postgres"* user to database (**database name may be different, check pgAdmin 4**)

![connection to Database](https://i.postimg.cc/3x4bdJZ9/7-connection-to-DB.png)

Then, enter database password (by default is: "1234" or "0000") .
And now we can receive, for example, the user "Corteza Service" or any other that exists in the DB.
If the output looks like this, the database data was retrieved successfully:

![data output](https://i.postimg.cc/B6shY5fw/8-data-output.png)

### Setting up the *DB_DSN* value in .env

Replace underlined values on your local:
 - user (username of database, by default is "postgres");
 - password (database password, by default is "1234" or "0000");
 - database (database name).
  
![ configure DB_DSN ](https://i.postimg.cc/HnvBkfcf/9-configure-connection-to-DB.png)

It might look like this: 

![ example DB_DSN ](https://i.postimg.cc/k41xQWJS/10-exemple-DB-DSN.png)

### 3. Authorization

As in the <a  href="#1-domain">Domain</a>  settings, **AUTH_BASE_URL** should be changed on the *localhost*, that you set already up in Domain and docker-compose.yaml (VIRTUAL_HOST).

![ example DB_DSN ](https://i.postimg.cc/L5GL7RZ6/11-auth-value.png)

# Running docker server

Now you should install the official [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/) and then inside your `corteza` directory (next to your `docker-compose.yaml` and `.env` files), run the docker compose, due to command: 
```bash	
docker-compose up -d
```
Launch the *localhost:8080* on your browser, and you will see Auth. page: 

![ Authorization page ](https://i.postimg.cc/vH6Wn1p1/12-auth-screen.png)

***Tip:***
*Use the docker commands according [the official documentation](https://docs.docker.com/reference/cli/docker/compose/)*

Log in **not with** Domain Credentials, but enter Email and corresponding Password users with admin permission. It might be **it-service** user.
After that you will have access to your local CRM system:

![ main page ](https://i.postimg.cc/8cfrL3n8/13-crm-main-page.png)

# Corteza-vue

## Bit bucket

Corteza-vue project may look like: 
<p align="center">
<img src="https://i.postimg.cc/52NLRHZ2/14-structure-vue-corteza.png" alt="HTML tutorial">  
</p>

## Configure dependencies  

Every one directory inside your `corteza-vue project` is **a separate local project** , that is supposed to be run independently  *on different localhosts*, for this reason we have to configure every project (admin, compose, etc.) **alone** but on **the same** way.	

### config.js

Let's configure for example **Admin** "project", other projects could be set up the same as this one. Inside `admin` directory move on **`public`** folder and create over there  **`config.js`** file.  Add into this file this code: 
```javascript
window.CortezaAPI = 'http://localhost:8080/api'
```
***Note:***
*Due to this command the authorization process will be redirected to local docker server, that is running. Because of <a  href="#running-docker-server">completed authorization</a> over there, here this process will be skipped.*

### package.json

According your current Corteza-vue version, you have to pay your attention to 
following dependencies:

![ dependencies ](https://i.postimg.cc/C5wMv8zh/15-dependencies.png)

***Important:***
**`All underlined packages must be at least the same version as the picture above, or newer.`**

### vue.config-builder.js

Check if the following values **lintOnSave** and **VERSION** (into configureWebpack > plugins) are the same as bellow: 
```javascript
...
return {
...
lintOnSave:  false, //must be false
...
configureWebpack: {
...
plugins: [
...
VERSION:  JSON.stringify(version || ('' + 'local dev').trim()), // check this value as well
...
```

### node

Install node dependencies on every project (admin, compose, etc.):
```bash	
npm istall
```

### building 

To build a project (one folder), a part of which will be mounted in docker server, use: 
```bash	
npm run build
```
Now **`dist`** folder has been updated. All compiled code used by the docker for frontend rendering is located here.

![ folder after built project](https://i.postimg.cc/C5XWdZ9C/16-after-building.png)



## sample of running local server

