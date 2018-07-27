# Setup instructions for OpenPaaS

If you encounter errors, have a quick look at [F.A.Q](https://github.com/Open-Up/workshop-ifi-27-07-2018/wiki/Setup-instructions-for-OpenPaaS#faq) part.

## I. Preparation.

### [Docker](https://www.docker.com/)
​
Follow documentation [here](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/) for installing docker.

Follow [Manage Docker as a non-root user](https://docs.docker.com/install/linux/linux-postinstall/) to run docker without root requirement.

### [Git](https://git-scm.com/)

### NodeJs

- Install nvm: following [here](https://github.com/creationix/nvm#installation)
- Install node 8:

```bash
    nvm install 8
    nvm use 8
    nvm alias default 8
```

### Additional packages

The following packages are for Ubuntu. If you are using other distributions of Linux, find the corresponding packages.

```bash
    sudo apt-get install build-essential python-setuptools graphicsmagick graphicsmagick-imagemagick-compat libcairo2-dev libpango1.0-dev libgif-dev
```

### Install the npm dependency
```bash
npm install -g mocha grunt-cli bower karma-cli
```
​
## II. Installation
    
### Clone OpenPaaS project
​
```bash
git clone https://ci.linagora.com/linagora/lgs/openpaas/esn.git
```
 
### Initiate services
​
Create and start docker containers for the required services:
​
```bash
docker run -d --name esn-elas -p 9200:9200 elasticsearch:2.3.2
docker run -d --name esn-mongo -p 27017:27017 mongo:3.2.0
docker run -d --name esn-redis -p 6379:6379 redis:latest
docker run -d --name esn-rabbit -p 5672:5672 rabbitmq:3.6.5-management
docker run --name esn-sabre -d -p 8001:80 -e "SABRE_MONGO_HOST=172.17.0.1" -e "ESN_MONGO_HOST=172.17.0.1" -e "ESN_HOST=172.17.0.1" -e "AMQP_HOST=172.17.0.1"  linagora/esn-sabre
```

And then during the development process, you can start or stop the services by:

```bash
# To stop
docker stop esn-elas esn-mongo esn-redis esn-rabbit esn-sabre
# To start
docker start esn-elas esn-mongo esn-redis esn-rabbit esn-sabre
```

### Go to the ESN project directory and install project dependencies
*Note*: You are expected to run the following commands as a normal user (Not as root user).

```bash
   cd esn
   npm install
   bower install
```
​

### Configuration
- **DB**

```bash
$ node ./bin/cli db --host localhost --port 27017 --database esn
```

- **elasticsearch**

It will create the indexes on the elasticsearch instance defined from CLI options.

```bash
$ node ./bin/cli elasticsearch -t users -i user.idx
```
- t (--type): Defines the type of index. When not set, it will create all the required indexes.
- i (--index): Defines the index to create. When not set, it will be built from type(Eg: if the type is __users__ then the index is __users.idx__.

- **populate sample user accounts**

```bash
$ node ./bin/cli populate
```

Once populated, you should be able to log into the OpenPaaS instance using user `admin@open-paas.org` and password `secret`.

### Start OpenPaas

- Start OpenPaaS in dev mode:

​
`$ grunt dev`
​

Open `http://localhost:8080` with your browser and login with `admin@open-paas.org/secret`


## F.A.Q.

> Got __EACCESS__ error when running `npm install`

Try to remove ~/.npm and rerun npm install
```bash
  rm -r ~/.npm
  npm install
```

> Got error relating to node-canvas

Following this [documentation](https://github.com/Automattic/node-canvas)

Example with ubuntu, try to run the following command:
```bash
  sudo apt-get install libcairo2-dev libjpeg8-dev libpango1.0-dev libgif-dev build-essential g++
```

> During further manipulations you encounter errors with node modules

Try to reinstall them

```bash
rm -rf node_modules/
npm i
```

> The docker containers port already taken by other services

You need to find out and stop/remove the services which are using the ports.

```bash
  sudo netstat -pna | grep port
  sudo service service_name stop
```
