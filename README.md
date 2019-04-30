# code-server

code-server docker-compose configurations.

## Table of Contents

- [Requirements](#requirements)
- [Usage](#usage)
- [Installation of Languages (optional)](#installation-of-languages-optional)
    - [Chef](#chef)
    - [Golang](#golang)
    - [Java](#java)
    - [Node.js](#nodejs)
    - [Python](#python)
    - [Ruby](#ruby)
- [Tools (optional)](#tools-optional)
    - [direnv](#direnv)
    - [jq (recommended)](#jq-recommended)
    - [yq](#yq)
    - [kubectl](#kubectl)
    - [Optware-ng (recommended)](#optware-ng-recommended)

## Requirements

- Docker
- Docker Compose

<details><summary>Docker & Docker Compose installation by Chef (optional)</summary><div>

```bash
Ubuntu:$ sudo apt-get install ca-certificates curl git
CentOS:$ sudo yum install ca-certificates curl git

$ curl -L https://omnitruck.chef.io/install.sh | sudo bash
$ git clone git://git.osdn.net/gitroot/metasearch/grid-chef-repo.git
$ cd grid-chef-repo/
$ sudo chef-client -z -c solo.rb -j nodes/local-docker.json
$ sudo docker info
```
</div></details>

## Usage

- ssh a host server.

- install configurations.

```bash
$ git clone https://github.com/wdstar/code-server.git
$ cd code-server
$ ./init
```

- start a code-server.

```bash
$ ./start
```

- check the access password.

```bash
$ ./logs
```

- access https://localhost:8443/ and enter the password.

- stop a code-server.

```bash
$ ./stop
```

Happy coding!

## Installation of Languages (optional)

### Chef

- https://downloads.chef.io/chefdk/

```bash
$ curl -L https://www.chef.io/chef/install.sh | sudo bash -s -- -P chefdk -v 1
$ eval "$(/opt/chefdk/bin/chef shell-init bash)"
$ chef -v
```

### Golang

- https://golang.org/
- [Official binary distributions](https://golang.org/dl/)

```bash
$ wget https://dl.google.com/go/go<version>.linux-amd64.tar.gz
$ sudo tar -C /opt -xzf go*.linux-amd64.tar.gz
$ echo 'PATH=${PATH}:/opt/go/bin' >> ~/.bashrc
$ . ~/.bashrc
$ /opt/go/bin/go version
```

### Java

- Install by the Optware-ng (see the following *Tools* section to install it)

```bash
$ sudo ipkg install openjdk8-jdk
$ java -version
```

### Node.js

- https://nodejs.org/
- [Node Version Manager](https://github.com/nvm-sh/nvm)

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
$ . ~/.bashrc
$ nvm install --lts
$ node -v
```

### Python

- Install by the Optware-ng (see the following *Tools* section to install it)

```bash
$ sudo ipkg update
$ sudo ipkg install python
$ python -V
or 
$ sudo ipkg install python3
$ python3 -V
```

- or Use deb packages (**Volatile!**)

```bash
$ sudo apt-get update
$ sudo apt-get install python
$ python -V
or
$ sudo apt-get install python3
$ python3 -V
```

### Ruby

- Install by the Optware-ng (see the following *Tools* section to install it)

```bash
$ sudo ipkg update
$ sudo ipkg install ruby
$ ruby -v
```

- or Use Chef DK embedded Ruby.

## Tools (optional)

### direnv

- https://github.com/direnv/direnv

```bash
$ sudo curl -o /opt/bin/direnv -L \
  $(curl -s -L https://api.github.com/repos/direnv/direnv/releases/latest | jq -r '.assets[] | select(.name | contains("linux-amd64")) | .browser_download_url')
$ sudo chmod +x /opt/bin/direnv
$ /opt/bin/direnv version

# add `eval "$(direnv hook bash)"` to ~/.bashrc
```

### jq (recommended)

- https://github.com/stedolan/jq

```bash
$ sudo curl -o /opt/bin/jq -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64
$ sudo chmod +x /opt/bin/jq
$ /opt/bin/jq -V
```

### yq

- https://github.com/mikefarah/yq

```bash
$ sudo curl -o /opt/bin/yq -L \
  $(curl -s -L https://api.github.com/repos/mikefarah/yq/releases/latest | jq -r '.assets[] | select(.name | contains("linux_amd64")) | .browser_download_url')
$ sudo chmod +x /opt/bin/yq
$ /opt/bin/yq -V
```

### kubectl

- https://kubernetes.io/docs/tasks/tools/install-kubectl/

```bash
$ sudo curl -L -o /opt/bin/kubectl \
  https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
$ sudo chmod +x /opt/bin/kubectl
$ /opt/bin/kubectl version
```

### Optware-ng (recommended)

- https://github.com/Optware/Optware-ng

```bash
$ wget -O - http://ipkg.nslu2-linux.org/optware-ng/bootstrap/buildroot-x86_64-bootstrap.sh | sudo sh
$ echo 'PATH=${PATH}:/opt/bin:/opt/sbin' >> ~/.bashrc
$ . ~/.bashrc
$ sudo ipkg update
$ sudo ipkg list
$ sudo ipkg install <package>
```
