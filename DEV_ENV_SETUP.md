### Mattermost
---
#### Setup the development server
---
##### Configure Docker CE

- Install and configure Docker CE using the instructions at [Docker installation](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
```sh
# Resume

# Setup the repository
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    software-properties-common
    
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo apt-key fingerprint 0EBFCD88

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
   
# Install Docker CE

$ sudo apt-get update

$ sudo apt-get install docker-ce

#Verify installation
$ sudo docker run hello-world
```
- Edit __/etc/hosts__ to include:
```sh
127.0.0.1     dockerhost
```
- Add username to the docker group
```sh
sudo gpasswd -a {username} docker
```
- Restart the docker daemon
```sh
sudo service docker restart
```
- Install the build-essential package
```sh
sudo apt-get install build-essential
```
- Download and install Go

```sh
wget https://storage.googleapis.com/golang/go1.9.4.linux-amd64.tar.gz

sudo tar -C /usr/local -xzf go1.9.4.linux-amd64.tar.gz
```
- Setup Go workspace:
```sh
mkdir ~/go
# Add these to ~/.bashrc file:

export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:/usr/local/go/bin
ulimit -n 8096

# Load the config in current terminal
source ~/.bashrc
```

- Setup the Mattermost directories
```sh
mkdir -p ~/go/src/github.com/mattermost
cd ~/go/src/github.com/mattermost

# clone the fork
git clone https://github.com/WAUIO/mattermost-server.git --depth=1
```
- __:warning: :warning: Important :warning: :warning: Logout then login again from your session__

- Stop your mysql service if you have locally running

```sh
sudo service mysql stop
```

- Startup and test env
```sh
cd mattermost-server
make run-server
make stop-server # stop the server after it starts succesfully
#Normally, server will start on http://localhost:8065
```


#### Useful server Makefile commands

- __make run__ will run the server, symlink your mattermost-webapp folder and start a watcher for the web app
- __make stop__ stops the server and the web app watcher
- __make run-server__ will run only the server and not the client
- __make debug-server__ will run the server in the delve debugger
- __make stop-server__ stops only the server
- __make clean-docker__ stops and removes your Docker images and is a good way to wipe your database
- __make run-fullmap__ will run the server and build the client with the full source map for easier debugging
- __make clean__ cleans your local environment of temporary files
- __make nuke__ wipes your local environment back to a completely fresh start
- __make package__ creates packages for distributing your builds and puts them in the ~/go/src/github.com/mattermost/mattermost-server/dist directory. First you will need to run make build and make build-webapp.

---
#### Setup guide for webapp
----

- Clone the repo for the webapp next to the mattermost-server directory in go/src/....

```sh
git clone https://github.com/WAUIO/mattermost-webapp.git --depth=1
```
- Install some dependencies which causes error sometimes if lib not installed
```sh
sudo apt-get install libpng16-dev
```