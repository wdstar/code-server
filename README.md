# code-server

code-server docker-compose configurations.

## Table of Contents

- [Requirements](#requirements)
    - [Docker & Docker Compose installation by Chef (optional)](#docker--docker-compose-installation-by-chef-optional)
- [Usage](#usage)
- [Installation of Languages (optional)](#installation-of-languages-optional)
    - [Chef](#chef)
    - [Golang](#golang)
    - [Node.js](#nodejs)
    - [Python](#python)
    - [Ruby](#ruby)
- [Tools (optional)](#tools-optional)
    - [direnv](#direnv)
    - [kubectl](#kubectl)
    - [Optware-ng](#optware-ng)

## Requirements

- Docker
- Docker Compose

### Docker & Docker Compose installation by Chef (optional)

```
Ubuntu:$ sudo apt-get install ca-certificates curl git
CentOS:$ sudo yum install ca-certificates curl git

$ curl -L https://omnitruck.chef.io/install.sh | sudo bash
$ git clone git://git.osdn.net/gitroot/metasearch/grid-chef-repo.git
$ cd grid-chef-repo/
$ sudo chef-client -z -c solo.rb -j nodes/local-docker.json
$ sudo docker info
```

## Usage

- ssh a host server.

- install configurations.

```
$ git clone https://github.com/wdstar/code-server.git
$ cd code-server
$ ./init
```

- start a code-server.

```
$ ./start
```

- check the access password.

```
$ ./logs
```

- access https://localhost:8443/ and enter the password.

- stop a code-server.

```
$ ./stop
```

Happy coding!

## Installation of Languages (optional)

### Chef

- https://downloads.chef.io/chefdk/

```
$ curl -L https://www.chef.io/chef/install.sh | sudo bash -s -- -P chefdk -v 1
$ eval "$(/opt/chefdk/bin/chef shell-init bash)"
$ chef -v
```

### Golang

- https://golang.org/
- [Official binary distributions](https://golang.org/dl/)

```
$ wget https://dl.google.com/go/go<version>.linux-amd64.tar.gz
$ sudo tar -C /opt -xzf go<version>.linux-amd64.tar.gz
$ /opt/go/bin/go version
```

### Node.js

- https://nodejs.org/
- [Node Version Manager](https://github.com/nvm-sh/nvm)

```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
$ nvm install --lts
$ . ~/.bashrc
$ node -v
```

### Python

- Install by the Optware-ng

```
$ sudo /opt/bin/ipkg update
$ sudo /opt/bin/ipkg install python
$ python -V
or 
$ sudo /opt/bin/ipkg install python3
$ python3 -V
```

- or Use deb packages (**Volatile!**)

```
$ sudo apt-get update
$ sudo apt-get install python
$ python -V
or
$ sudo apt-get install python3
$ python3 -V
```

### Ruby

- Install by the Optware-ng

```
$ sudo /opt/bin/ipkg update
$ sudo /opt/bin/ipkg install ruby
$ ruby -v
```

- or Use Chef DK embedded Ruby.

## Tools (optional)

### direnv

- https://github.com/direnv/direnv

```
$ sudo curl -L -o /opt/bin/direnv https://github.com/direnv/direnv/releases/download/<version>/direnv.linux-amd64
$ sudo chmod +x /opt/bin/direnv
$ /opt/bin/direnv version

# add `eval "$(direnv hook bash)"` to ~/.bashrc
```

### kubectl

- https://kubernetes.io/docs/tasks/tools/install-kubectl/

```
$ sudo curl -L -o /opt/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
$ sudo chmod +x /opt/bin/kubectl
$ /opt/bin/kubectl version
```

### Optware-ng

- https://github.com/Optware/Optware-ng

```
$ wget -O - http://ipkg.nslu2-linux.org/optware-ng/bootstrap/buildroot-x86_64-bootstrap.sh | sudo sh
$ sudo /opt/bin/ipkg update
$ sudo /opt/bin/ipkg list
$ sudo /opt/bin/ipkg install <package>
```
