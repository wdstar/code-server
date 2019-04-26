# code-server

code-server docker-compose configurations.

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
$ node -v
```
