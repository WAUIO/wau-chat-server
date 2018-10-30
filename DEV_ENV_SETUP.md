### Mattermost
---
#### Setup the development server
---
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
git clone https://github.com/WAUIO/mattermost-server.git
```
---
#### Setup mysql
---
```sh
mysql -u root -p
mysql> ALTER USER "root"@"localhost" IDENTIFIED BY "mmuser-password";
mysql> create user "mmuser"@"%" identified by "mmuser-password";
mysql> create database mattermost;
mysql> grant all privileges on mattermost.* to "mmuser"@"%";
mysql> exit
```
- Then change this line in file config/config.json:120
```js
mmuser:mmuser-password@tcp(localhost:3306)/mattermost_test?charset=utf8mb4,utf8\u0026readTimeout=30s\u0026writeTimeout=30s
```

---
#### Setup guide for webapp
----

- Clone the repo for the webapp next to the mattermost-server directory in go/src/....

```sh
git clone https://github.com/WAUIO/mattermost-webapp.git
```
- Install some dependencies which causes error sometimes if lib not installed
```sh
sudo apt-get install libpng16-dev
```

---
#### Build the server
---
```sh
make build-client
make build-linux
```

You then can launch the server
