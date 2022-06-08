---
sidebar_position: 2
---

# How to install and deploy the development environment.

:exclamation: This project contains 8 containers which communicates together. The Spring Cloud technology is very tedious to install and I have had a lot of trouble and difficulties with it. I have never been able to run any spring cloud project that I have used to learn this technology without hours spent fixing configuration issues.

<hr>

## UPDATE

The first version of this tutorial is based on a version of this project that doesn't run on a local environment. I have made an update of this project the day after the deadline in order to provide a fully working version of this project. I know I went beyond the deadline but I was very frustrated that after hundreds of hours spent on this project I was not able to deliver a working version even if it worked on my production environment and my laptop.

I am fully aware that I can be penalized for going beyond the deadline and fully accept your decision to accept this updated version or not. I have kept both versions (the orginal non working and the new version) in seperate Docker images and I put my updates on a seperate demo branch in order to be fully transparent about what has been modified. I haven't added any new features, I have just reworked the configuration to make it function.

Below I kept the first version of the tutorial for the non working version and I have added a new tutorial for the working version.

Thank you.

<hr>

**Required**

- Ubuntu 20+LTS with sudo rigths
- Docker and docker compose
- Git
- Internet

## Tutorial version 1 (non working version)

### Step 1

Install your server with the above requirements. The VM setup guide can help you, but it is overkill as it has been writen to deploy a production server.

### Step 2

In the user folder : /home/username

#### Clone the configuration repository

This repository is private so you should send an access request, OR I can send you a zip folder.

With https

```bash
$ git clone https://github.com/AdventureForge/cloud-configuration.git
```

OR with SSH

```bash
$ git@github.com:AdventureForge/cloud-configuration.git
```

#### Clone the API repository

With https

```bash
$ git clone https://github.com/AdventureForge/api.git
```

OR with SSH

```bash
$ git clone git@github.com:AdventureForge/api.git
```

#### Project organization

Your user directory should look like this:

```bash
home
     |_ <username>
                   |_ cloud_configuration
                   |_ api
```

:exclamation: **This is VERY important to follow this organization.**

### Step 3

#### Add aliases to your Hosts.

cd and open your hosts file

```bash
$ cd /etc/
$ sudo nano hosts
```

Add the alias bellow to the 127.0.0.1 line

```
127.0.0.1 localhost gateway keycloak
```

This ensure that the react application can communicate with the Docker private network

### Step 4

#### Start the project with the docker compose

```bash
$ cd ~/api/docker/dev/
$ docker-compose up
```

If it works properly, the applications take few minutes to deploy and connect to each other. If it doesn't works, you will see big stack traces in the logs. this is because the services are not able to find the configuration server, which store the configuration of all the microservices, and therefore, are not able to configure themeselves.

At this point you will know whether you will be able to test the application or not...

### Step 5

#### If, by miracle, it works

There are still few steps.

If the keycloak has succeded to import its settings, the database will setup.
You need to update a the private key used by the services to authentify themselves to Keycloak in the database, because the graphical interface doesn't allow to do it.

The docker container does not allow to access the database from inside the container (which you cloud access with the command : docker exec -ti adventureforge-postgres bash). So you need a database editor like Datagrip or DBeaver.

##### Update client secret

To login to the dabase:

```
url: jdbc:postgresql://database:5432/adventureforgedb
user: postgres
password: postgres
```

Go to the table **client** and update the column **secret** with the value **yoCZsQZzFHJpg3i44jJgRblgzDq87e9n** for the client_id **adventureforge_back**

### Step 6

##### Create a user with admin rights

In order to test the full application, you need a user with admin rights. The default keycloak admin provided is not able to login to the application.

Go to the keyclaok administration page which is available at [http://keycloak:8080/auth](http://keycloak:8080/auth)

The default admin user is:

```
user: admin
password: admin
```

then go to

```
Users > Add user
```

fill the required fields.

:exclamation: **Toggle** **Email Verified** to **ON**

Save. you should be redirected to the page with you new user.

- Got to Credentials, fill the password fields, **Toggle Temporary OFF**, click on set password. validate.
- Go to the **Role Mappings** tab. Click on ADMIN in the available roles and then click on **Add selected**.

Now you can use the application.

It is available at [http://localhost:3000](http://localhost:3000)

# Tutorial version 2 (working version)

### Step 1

Install your server with the above requirements. The VM setup guide can help you, but it is overkill as it has been writen to deploy a production server.

### Step 2

In the user folder : /home/username

#### Clone the API repository

With https

```bash
$ git clone https://github.com/AdventureForge/api.git
```

OR with SSH

```bash
$ git clone git@github.com:AdventureForge/api.git
```

Checkout demo branch

```
$ git fetch origin demo
$ git checkout demo
```

### Step 3

#### Add aliases to your Hosts.

cd and open your hosts file

```bash
$ cd /etc/
$ sudo nano hosts
```

Add the alias bellow to the 127.0.0.1 line

```
127.0.0.1 localhost gateway keycloak
```

This ensure that the react application can communicate with the Docker private network

### Step 4

#### Start the project with the docker compose

```bash
$ cd ~/api/docker/demo/
$ docker-compose up
```

If it works properly, the applications take few minutes to deploy and connect to each other, deploy Keycloak and setup the database.

### Step 5

There are still few steps.

If the keycloak has succeded to import its settings, the database will setup.
You need to update a the private key used by the services to authentify themselves to Keycloak in the database, because the graphical interface doesn't allow to do it.

The docker container does not allow to access the database from inside the container (which you cloud access with the command : docker exec -ti adventureforge-postgres bash). So you need a database editor like Datagrip or DBeaver.

##### Update client secret

To login to the dabase:

```
url: jdbc:postgresql://database:5432/adventureforgedb
user: postgres
password: postgres
```

Go to the table **client** and update the column **secret** with the value **yoCZsQZzFHJpg3i44jJgRblgzDq87e9n** for the client_id **adventureforge_back**

### Step 6

##### Create a user with admin rights

In order to test the full application, you need a user with admin rights. The default keycloak admin provided is not able to login to the application.

Go to the keyclaok administration page which is available at [http://keycloak:8080/auth](http://keycloak:8080/auth)

The default admin user is:

```
user: admin
password: admin
```

then go to

```
Users > Add user
```

fill the required fields.

:exclamation: **Toggle** **Email Verified** to **ON**

Save. you should be redirected to the page with you new user.

- Got to Credentials, fill the password fields, **Toggle Temporary OFF**, click on set password. validate.
- Go to the **Role Mappings** tab. Click on ADMIN in the available roles and then click on **Add selected**.

Now you can use the application.

It is available at [http://localhost:3000](http://localhost:3000)
