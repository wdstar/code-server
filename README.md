# code-server

code-server local execution scripts and docker-compose configurations.

## Table of Contents

- [on Local server](#on-local-server)
- [on Docker](#on-docker)
    - [Requirements](#requirements)
    - [Persistent Volumes](#persistent-volumes)
    - [Environment Variables](#environment-variables)
    - [Usage](#usage)
- [Installation of Languages (optional)](#installation-of-languages-optional)
    - [C#](#c)
    - [Chef](#chef)
    - [Golang](#golang)
    - [Java](#java)
    - [Node.js](#nodejs)
    - [Perl](#perl)
    - [Python](#python)
    - [Ruby](#ruby)
    - [Rust](#rust)
    - [TypeScript](#typescript)
- [Tools (optional)](#tools-optional)
    - [Byobu](#byobu)
    - [direnv](#direnv)
    - [Docker CLI](#docker-cli)
    - [Docker Compose](#docker-compose)
    - [Gradle](#gradle)
    - [helm](#helm)
    - [jq (recommended)](#jq-recommended)
    - [kubectl](#kubectl)
    - [Optware-ng (recommended)](#optware-ng-recommended)
    - [yarn](#yarn)
    - [yq](#yq)
- [Help](#help)
    - [v2.x](#v2x)
    - [v1.x](#v1x)

## on Local server

- install `curl` and `jq` commands.
- install the code-server.

```bash
$ git clone https://github.com/wdstar/code-server.git
$ cd code-server/local
$ ./code-server_install
```

- start a code-server.

```bash
$ ./coded
```

- access http://localhost:8080/ and enter the password.

- stop a code-server by hitting `Ctrl+C`.

Happy coding!

## on Docker

### Requirements

- Docker
- Docker Compose
- sudo privileges
- ssh-agent (optional but recommended)

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

### Persistent Volumes

|Volumes|Mounted Path|Descriptions|
|:---|:---|:---|
|`./docker-compose/coder`|`/home/coder`|`coder` default user's home directory.|
|`./docker-compose/opt`|`/opt`|To install language runtimes, SDKs and tools.|

### Environment Variables

- You can customize the environment variables in a code-server container by modifing the `./docker-compose/.env` file.

|Env. var. names in container|Env. var. names in the `./docker-compose/.env` file|Default values|Remarks|
|:---|:---|:---|:---|
|`GOROOT`|`CS_GOROOT`|`/opt/go`||
|`GOPATH`|`CS_GOPATH`|`/home/coder/go`||
|`LD_LIBRARY_PATH`|`CS_LD_LIBRARY_PATH`|`/opt/icu/lib`|for ICU|
|`PATH`|`CS_PATH`|`/opt/bin:/opt/sbin:/opt/dotnet:/home/coder/dotnet:/opt/go/bin`|for Optware-ng, .Net, Go|

### Usage

- ssh a host server.

- install configurations.

```bash
$ git clone https://github.com/wdstar/code-server.git
$ cd code-server/docker-compose
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

- execute Bash.

```
$ ./exec-bash
```

- stop a code-server.

```bash
$ ./stop
```

Happy coding!

## Installation of Languages (optional)

### C#

- https://dotnet.microsoft.com/download

#### on Local Ubuntu (18.04 LTS)

```bash
$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
$ sudo dpkg -i packages-microsoft-prod.deb
$ sudo add-apt-repository universe
$ sudo apt-get install apt-transport-https
$ sudo apt-get update
$ sudo apt-get install dotnet-sdk-2.2
```

#### on Docker

- [How to use .NET Core on RHEL 6 / CentOS 6](https://github.com/dotnet/core/blob/master/Documentation/build-and-install-rhel6-prerequisites.md)
- https://github.com/unicode-org/icu/releases/

```bash
# download .NET Core binaries.
$ mkdir ${HOME}/dotnet
$ tar xvzf dotnet-sdk-*-linux-x64.tar.gz -C ${HOME}/dotnet
$ echo 'DOTNET_ROOT=${HOME}/dotnet' >> ~/.bashrc
$ echo 'PATH=$PATH:${HOME}/dotnet' >> ~/.bashrc
# optional
$ echo 'DOTNET_CLI_TELEMETRY_OPTOUT=1' >> ~/.bashrc

$ curl -L -O https://github.com/unicode-org/icu/releases/download/release-64-2/icu4c-64_2-Ubuntu18.04-x64.tgz
$ sudo mkdir /opt/icu
$ sudo tar -xvzf icu4c-64_2-Ubuntu18.04-x64.tgz -C /opt/icu --strip-components=4
$ echo 'LD_LIBRARY_PATH=/opt/icu/lib' >> ~/.bashrc

$ . ~/.bashrc
$ dotnet --version
```

### Chef

- https://downloads.chef.io/chefdk/

#### on Local Ubuntu & Docker

```bash
$ curl -L https://www.chef.io/chef/install.sh | sudo bash -s -- -P chefdk -v 1
$ eval "$(/opt/chefdk/bin/chef shell-init bash)"
$ chef -v
```

### Golang

- https://golang.org/
- [Official binary distributions](https://golang.org/dl/)

#### on Local Ubuntu

```bash
$ wget https://dl.google.com/go/go<version>.linux-amd64.tar.gz
$ sudo tar -C /usr/local -xzf go*.linux-amd64.tar.gz
$ echo 'PATH=${PATH}:/usr/local/go/bin' >> ~/.bashrc
$ . ~/.bashrc
$ go version
```

#### on Docker

```bash
$ wget https://dl.google.com/go/go<version>.linux-amd64.tar.gz
$ sudo tar -C /opt -xzf go*.linux-amd64.tar.gz
$ echo 'PATH=${PATH}:/opt/go/bin' >> ~/.bashrc
$ . ~/.bashrc
$ go version
```

- Known issue: `exec: "gcc": executable file not found`
  - Error
    ```
    $ go test ./...
    # runtime/cgo
    exec: "gcc": executable file not found in $PATH
    ```
  - Solution
    ```bash
    $ export CGO_ENABLED=0
    or
    $ sudo ipkg install gcc
    ```

### Java

#### on Local Ubuntu

```bash
$ sudo apt-get install default-jdk
# or
$ sudo apt-get install openjdk-<major_version>-jdk
$ java -version
```

#### on Docker

- Install by the Optware-ng (see the following *Tools* section to install it)
    ```bash
    $ sudo ipkg install openjdk8-jdk
    $ java -version
    ```

- Other distributions
    - https://adoptopenjdk.net/

### Node.js

- https://nodejs.org/
- [Node Version Manager](https://github.com/nvm-sh/nvm)

#### on Local Ubuntu & Docker

```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
$ . ~/.bashrc
$ nvm install --lts
$ node -v
```

### Perl

#### on Local Ubuntu & Docker

Pre-installed.

```bash
$ perl -v
```

### Python

#### on Local Ubuntu

- Use deb packages

```bash
$ sudo apt-get install python
$ python -V
or
$ sudo apt-get install python3
$ python3 -V
```

#### on Docker

- Install by the Optware-ng (see the following *Tools* section to install it)

```bash
$ sudo ipkg update
$ sudo ipkg install python
$ python -V
or 
$ sudo ipkg install python3
$ python3 -V
```

### Ruby

#### on Local Ubuntu

```bash
$ sudo apt-get install ruby
$ ruby -v
```

- or Use Chef DK embedded Ruby.

#### on Docker

- Install by the Optware-ng (see the following *Tools* section to install it)

```bash
$ sudo ipkg update
$ sudo ipkg install ruby
$ ruby -v
```

- or Use Chef DK embedded Ruby.

### Rust

- https://www.rust-lang.org/tools/install

#### on Local Ubuntu & Docker

```bash
$ curl https://sh.rustup.rs -sSf | sh
...
Rust is installed now. Great!

To get started you need Cargo's bin directory ($HOME/.cargo/bin) in your PATH
environment variable. Next time you log in this will be done automatically.

To configure your current shell run source $HOME/.cargo/env
$ source $HOME/.cargo/env
$ rustc --version
```

### TypeScript

- Install Node.js (see the above section) and then

#### on Local Ubuntu & Docker

```
$ mkdir your-project
$ cd your-project
$ npm init
$ npm install --save-dev typescript
$ npx tsc -v
```

## Tools (optional)

### Byobu

#### on Local Ubuntu

```bash
$ sudo apt-get install byobu
```

#### on Docker

- http://byobu.co/downloads.html

- Install `tmux` or `screen` by the Optware-ng (see the following *Optware-ng* section to install it).
```bash
$ sudo ipkg install tmux
# or
$ sudo ipkg install screen
```

- Install `byobu`.
```bash
$ sudo apt-get update
$ sudo apt-get install build-essential
$ wget https://launchpad.net/byobu/trunk/5.127/+download/byobu_5.127.orig.tar.gz
$ tar xvzf byobu*.tar.gz
$ cd byobu-*/
$ ./configure --prefix="$HOME/byobu"
$ echo "export BYOBU_PYTHON='/usr/bin/env python'" >> $HOME/.bashrc
$ make
$ make install
$ echo 'export PATH=$HOME/byobu/bin:$PATH' >> $HOME/.bashrc
$ . $HOME/.bashrc
$ byobu -v
```

### direnv

- https://github.com/direnv/direnv

#### on Local Ubuntu

```bash
$ sudo apt-get install direnv
```

#### on Docker

```bash
$ sudo curl -o /opt/bin/direnv -L \
  $(curl -s -L https://api.github.com/repos/direnv/direnv/releases/latest | jq -r '.assets[] | select(.name | contains("linux-amd64")) | .browser_download_url')
$ sudo chmod +x /opt/bin/direnv
$ /opt/bin/direnv version

# add `eval "$(direnv hook bash)"` to ~/.bashrc
```

### Docker CLI

- https://docs.docker.com/install/linux/docker-ce/binaries/

#### on Local Ubuntu

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce-cli 
```

#### on Docker

```bash
$ curl -L -O https://download.docker.com/linux/static/stable/x86_64/docker-18.09.5.tgz
$ sudo tar xvzf docker-*.tgz -C /opt/bin/ --strip=1 --wildcards '*/docker'
$ sudo docker version
```

### Docker Compose

- https://docs.docker.com/compose/install/

#### on Local Ubuntu

```bash
$ sudo curl -o /usr/local/bin/docker-compose -L \
  $(curl -s -L https://api.github.com/repos/docker/compose/releases/latest | jq -r '.assets[] | select(.name | endswith("Linux-x86_64")) | .browser_download_url')
$ sudo chmod +x /usr/local/bin/docker-compose
$ sudo docker-compose version
```

#### on Docker

```bash
$ sudo curl -o /opt/bin/docker-compose -L \
  $(curl -s -L https://api.github.com/repos/docker/compose/releases/latest | jq -r '.assets[] | select(.name | endswith("Linux-x86_64")) | .browser_download_url')
$ sudo chmod +x /opt/bin/docker-compose
$ sudo docker-compose version
```

### Gradle

- https://gradle.org/install/

```bash
$ curl -LO https://services.gradle.org/distributions/gradle-5.5.1-bin.zip
$ sudo unzip -d /opt gradle-*-bin.zip
$ sudo ln -s /opt/gradle-* /opt/gradle
```

### helm

- https://helm.sh/docs/using_helm/#installing-helm

#### on Local Ubuntu

```bash
$ curl -L https://git.io/get_helm.sh | bash
```

#### on Docker

```bash
$ curl -L https://git.io/get_helm.sh | bash
$ sudo mv $(which helm) /opt/bin/
$ /opt/bin/helm version
```

### jq (recommended)

- https://github.com/stedolan/jq

#### on Local Ubuntu

```bash
$ sudo apt-get install jq
```

#### on Docker

```bash
$ sudo curl -o /opt/bin/jq -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64
$ sudo chmod +x /opt/bin/jq
$ /opt/bin/jq -V
```

### kubectl

- https://kubernetes.io/docs/tasks/tools/install-kubectl/

#### on Local Ubuntu

```bash
$ sudo apt-get update && sudo apt-get install apt-transport-https
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ echo 'deb https://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install kubectl
$ kubectl version -c
```

#### on Docker

```bash
$ sudo curl -L -o /opt/bin/kubectl \
  https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
$ sudo chmod +x /opt/bin/kubectl
$ /opt/bin/kubectl version -c
```

### Optware-ng (recommended)

- https://github.com/Optware/Optware-ng

#### on Local Ubuntu & Docker

```bash
$ wget -O - http://ipkg.nslu2-linux.org/optware-ng/bootstrap/buildroot-x86_64-bootstrap.sh | sudo sh
$ echo 'PATH=${PATH}:/opt/bin:/opt/sbin' >> ~/.bashrc
$ . ~/.bashrc
$ sudo ipkg update
$ sudo ipkg list
$ sudo ipkg install <package>
```

### yarn

- https://yarnpkg.com/en/docs/install#alternatives-stable

#### on Local Ubuntu & Docker

```bash
$ sudo apt-get update
$ sudo apt-get install gpg
$ curl -o- -L https://yarnpkg.com/install.sh | bash
$ . ~/.bashrc
$ yarn -v
```

### yq

- https://github.com/mikefarah/yq

#### on Local Ubuntu

```bash
$ sudo curl -o /usr/local/bin/yq -L \
  $(curl -s -L https://api.github.com/repos/mikefarah/yq/releases/latest | jq -r '.assets[] | select(.name | contains("linux_amd64")) | .browser_download_url')
$ sudo chmod +x /usr/local/bin/yq
$ yq -V
```

#### on Docker

```bash
$ sudo curl -o /opt/bin/yq -L \
  $(curl -s -L https://api.github.com/repos/mikefarah/yq/releases/latest | jq -r '.assets[] | select(.name | contains("linux_amd64")) | .browser_download_url')
$ sudo chmod +x /opt/bin/yq
$ /opt/bin/yq -V
```

## Help

### v2.x

```
$ code-server --version
info  2.1665-vsc1.39.2
info  3d5db8313a91f80dfc402450fd117b4e63f7711e
info  x64

$ code-server --help
code-server 2.1665-vsc1.39.2

Usage: code-server [options][paths...]

Options
  --locale <locale>              The locale to use (e.g. en-US or zh-TW).
  --user-data-dir <dir>          Specifies the directory that user data is kept in. Can be used to open multiple distinct instances of Code.
  -v --version                   Print version.
  -h --help                      Print usage.
  --telemetry                    Shows all telemetry events which VS code collects.
  --extra-builtin-extensions-dir Path to an extra builtin extension directory.
  --extra-extensions-dir         Path to an extra user extension directory.
  --base-path                    Base path of the URL at which code-server is hosted (used for login redirects).
  --cert                         Path to certificate. If the path is omitted, both this and --cert-key will be generated.
  --cert-key                     Path to the certificate's key if one was provided.
  --format                       Format for the version. Allowed value is 'json'.
  --host                         Host for the server.
  --auth                         The type of authentication to use. Allowed values are 'password', 'none'.
  --open                         Open in the browser on startup.
  --port                         Port for the main server.
  --socket                       Listen on a socket instead of host:port.

Extensions Management
  --extensions-dir <dir>                            Set the root path for extensions.
  --list-extensions                                 List the installed extensions.
  --show-versions                                   Show versions of installed extensions, when using --list-extension.
  --category                                        Filters installed extensions by provided category, when using --list-extension.
  --install-extension <extension-id | path-to-vsix> Installs or updates the extension. Use `--force` argument to avoid prompts.
  --uninstall-extension <extension-id>              Uninstalls an extension.
  --enable-proposed-api <extension-id>              Enables proposed API features for extensions. Can receive one or more extension IDs to enable individually.

Troubleshooting
  --verbose                          Print verbose output (implies --wait).
  --log <level>                      Log level to use. Default is 'info'. Allowed values are 'critical', 'error', 'warn', 'info', 'debug', 'trace', 'off'.
  -s --status                        Print process usage and diagnostics information.
  --disable-extensions               Disable all installed extensions.
  --disable-extension <extension-id> Disable an extension.
```

### v1.x

```
$ code-server --version
1.1156-vsc1.33.1

$ code-server --help
Usage: code-server [options]

Run VS Code on a remote server.

Options:
  -V, --version                         output the version number
  --cert <value>
  --cert-key <value>
  -e, --extensions-dir <dir>            Override the main default path for user extensions.
  --extra-extensions-dir [dir]          Path to an extra user extension directory (repeatable). (default: [])
  --extra-builtin-extensions-dir [dir]  Path to an extra built-in extension directory (repeatable). (default: [])
  -d --user-data-dir <dir>              Specifies the directory that user data is kept in, useful when running as root.
  --data-dir <value>                    DEPRECATED: Use '--user-data-dir' instead. Customize where user-data is stored.
  -h, --host <value>                    Customize the hostname. (default: "0.0.0.0")
  -o, --open                            Open in the browser on startup.
  -p, --port <number>                   Port to bind on. (default: 8443)
  -N, --no-auth                         Start without requiring authentication.
  -H, --allow-http                      Allow http connections.
  -P, --password <value>                DEPRECATED: Use the PASSWORD environment variable instead. Specify a password for authentication.
  --disable-telemetry                   Disables ALL telemetry.
  --socket <value>                      Listen on a UNIX socket. Host and port will be ignored when set.
  --install-extension <value>           Install an extension by its ID.
  --bootstrap-fork <name>               Used for development. Never set.
  --extra-args <args>                   Used for development. Never set.
  -h, --help                            output usage information
```
