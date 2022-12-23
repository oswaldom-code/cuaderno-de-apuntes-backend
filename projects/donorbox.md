# DONORBOX
## _API - Sistema de gestión de donaciones_


Here in small summary of the project

## Features

- Here the list of features supported by software


## Tech

Dillinger uses a number of open source projects to work properly:

- [Ryby](https://www.ruby-lang.org/en/) - A dynamic, open source programming language with a focus on simplicity and productivity.
- [Rails](https://rubyonrails.org/) - Rails is a server-side web application framework written in Ruby.
- [Docker](https://www.docker.com/) - Docker is a set of platform as a service product that use OS-level virtualization to deliver software in packages called containers

## 🔧  Server configuration
The steps to follow for configuring the server with OS Ubuntu are listed below.
> Note: For local development environment you can skip steps 1 and 2

#### 1. Connect to server
To connect to the server for the first time you need to have the .pem file, which must be supplied by the project manager. Once you have the .pem file, you can connect to the server by running the following command:
```ssh
ssh {{USER}}@{{SERVER_IP_ADDRESS}} -i ./{{file_name}}.pem
```
#### 2. Add public keys of the  dev team
In order to facilitate the access of the development team to the server, the use of the .pem file can be omitted. To do this you must add the public keys of the developers to the /home/{{USER}}/.ssh/authorized_keys file.
> Note: The following tags must be replaced by their corresponding values.
`{{USER}}` The name of the user created to administer the server.
`{{SERVER_IP_ADDRESS}}` The IP address of the server
`{{file_name}}` The name of the .pem file supplied by the project manager

#### 3. Install git
```sh
sudo apt install git
```
verify correct git installation
```sh
git --version
```
#### 4. Install task
```sh
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d
```
verify correct task installation
```sh
task --version
```
or
```sh
./bin/task --version
```
> Note:  For more information on how to install task see [here](https://taskfile.dev/installation/)
####  5. Install docker 
To install docker it is necessary to run the following commands:
```sh
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
```sh
sudo mkdir -p /etc/apt/keyrings
``` 
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
``` 
 ```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
``` 
verify correct docker installation
```sh
sudo docker run hello-world 
```

 > Note:  For more information on how to install docker see [here](https://docs.docker.com/engine/install/ubuntu/)

####  6. Install docker-compose
```sh
sudo apt install docker-compose
```

### 🚀 Start up

DONORBOX API requires:
Make sure you have the following tools installed at the listed versions or higher. Otherwise you can take a look at the previous step Server configuration
- [Task](https://taskfile.dev/) v3+
- [Docker](https://www.docker.com/) v20+
- [Docker-compose](https://docs.docker.com/compose/) v1.25+

### 1. Set up the environment variables:

As the first step to launch the application, it is necessary to set the environment variables. To do this we must edit three files:
- [ROOT_PROJECT_PATH]/Taskfile.yml
- [ROOT_PROJECT_PATH]/config/environments/env.yaml
- [ROOT_PROJECT_PATH]/config/environments/{execution mode: (dev/prod/test)}/.env

##### Taskfile.yml:
The [Taskfile.yml](https://github.com/dataf3l/stock-donation-project3-backend/blob/main/Taskfile.yml) file contains a set of command shortcuts that will be very useful to us and which we will talk about in more detail later, but for now we are interested in configuring the execution environment in which the project will be run, since according to said configuration the tool will load the corresponding environment variables.
The variable [ENV](https://github.com/dataf3l/stock-donation-project3-backend/blob/5f3446bd78cbe2d8b65620f934bf7b507bcdda9c/Taskfile.yml#L6) has three possible values:
- `dev` -  To run the project in development mode.
- `prod` - To run the project in production mode.
- `test`- To run the project in test mode.

NOTE: Make sure ENV is set to dev

##### env.yaml:
In this file, configure the parameters and secrets necessary for the proper functioning of the application. Because of its sensitive content, this file is not found in the repository, but if [env.example.yaml](https://github.com/dataf3l/stock-donation-project3-backend/blob/main/config/environments/env.example.yaml) is found which contains a complete structure, it will give you a clear idea of the actual env.yaml file. 

Please contact the backend team to get an env.yaml file with all the secrets and other parameters.

##### .env:
The [.env](https://github.com/dataf3l/stock-donation-project3-backend/blob/main/config/environments/dev/.env.example) it currently contains the necessary parameters to run the application in development mode, so it is not necessary to make any additional changes. But we refer to it since to run the application in production it will be necessary to change the corresponding secret keys. 
Then just copy and paste the .env.example file and name the new file .env, preserving its location

### 2. Build the Docker images:
You need to build two images:
- `donorbox/ruby3-1:ubuntu-focal` -  It is an image based on Ubuntu 20.04 with ruby ​​and rails installed.
-  `donorbox/app:v1.0` -  The application image


You may be asking yourself this question: why two images and not one? Well, if you ask yourself that question, it is evident that you have little time developing with a stack that includes Docker. And the answer is that a lot of time is wasted every time you make a change in the application and run the containers to visualize the change made. Doing it this way is unproductive.

For this reason we built a first image with the Operating System, Ruby and Rails. Which corresponds to the heaviest part of processing by Docker and from this image, build the image of the application. This workflow saves a lot of time when developing.

Now, let's get down to work and build the images we need; To do this we will use the command shortcuts defined in the [Taskfile.yml](https://github.com/dataf3l/stock-donation-project3-backend/blob/main/Taskfile.yml) file, we execute the following commands:

```
task docker.build-images
```

This command will take care of building the containers described above.

Now we only need to start the application counter and import the database. We do that in the next step.

### 3. Run the application and import the database:
We are now in the final steps to have an instance of our application. We just have to run the following commands:

```
task app.up
```
This command will start two Docker containers, one with our application and one with a Postgres image for persistence.


The next step is to import our database, for this we run the following command:

```
task db.restore -- 2022-11-16_full_dump.sql
```
The 2022-11-16_full_dump.sql argument is you can change to the backup of your choice, but as of the writing of this documentation 2022-11-16_full_dump.sql is the file we are working with.

After successfully importing the test database, we can now make some requests to our API. To do this we suggest importing the [postman collection](https://github.com/dataf3l/stock-donation-project3-backend/blob/main/swagger/stock-donation-v1.postman_collection.json) that is in the project and that has the currently supported paths.

> Note If when testing the application you get an error due to pending migrations. Run the following command:
`task db.migrate`



## 🔨 Development script
In order to simplify the execution of somewhat complex commands, this project relies on a tool called task. The task cli tool is a tool that allows you to run commands in a simple way, it is used to run the project in development mode, run the tests, run the linter, etc.

## List of commands available

- `task help` - Show the list of commands available
- `task -l` - Show the list of commands available
- `task app.up ` - Run the containers in the background
- `task app.down` - Stop the containers
- `task db.full-dump` - Create a full dump of the database
- `task db.data-dump` - Restore the database with the data dump
- `task db.schema-dump` - Restore the database with the schema dump
- `task db.reset` - Reset the database
- `task db.schema-restore` - Restore the database with the schema dump
- `task db.migrate` - Run pending migrations
- `task docker.build-images` - Build the images of the containers of the project
- `task swagger.doc` - Generate the documentation of the API
- `task git.pull -- branch_name` - Update project to specific branch









