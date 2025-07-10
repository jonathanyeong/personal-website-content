---
pubDate: 2024-06-30
updatedDate: 2025-07-10
title: Setting up dev containers in Rails
description: Development containers are an alternative to local development. You can have a dev ready environment running in minutes. Here's how we can set one up in Rails, issues I ran into along the way, and my initial thoughts.
featured: true
draft: false
topics:
  - tutorial
---

[Development containers](https://containers.dev/), aka dev containers, have been around since 2019. Made by Microsoft, they allow you to use a container as a development environment. If you've used GitHub Codespaces, you've loaded a dev container. In Rails 7.2, there will be a new `devcontainer` flag that will generate a dev container folder for you. Ever since I found out about Rails devcontainers at a local Ruby meetup, I've been eager to try them. If you also want to be an early adopter, here are a few issues I ran into.
## Installing libraries
Before I can get dev containers working locally, I need to install a few libraries and extensions.

[VSCode dev container extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers), which will run my application as a dev container inside VSCode.

A docker runtime, that will run my devcontainer. If you're on a personal machine, [Orbstack](https://orbstack.dev/) (which I use) or [Docker Desktop](https://www.docker.com/products/docker-desktop/) is a simple way to get started. Or if you're on a work machine, [Colima](https://github.com/abiosoft/colima) is another option.

If you were to use Colima, you'll have to follow these steps:

`brew install colima`

Docker to run my containers.

`brew install docker`

Docker compose, which is needed to run the dev container `compose.yaml` file.

`brew install docker-compose`

I also updated my `~/.docker/config.json` to add the plugin

```json
"cliPluginsExtraDirs": [
	"/opt/homebrew/lib/docker/cli-plugins"
]
```

## Setting up a Rails dev container

If you have Rails 7.2+ installed, you can create create a new Rails app with a devcontainer:

```bash
rails new dev-containers-app --devcontainer
```

After opening the project in VSCode, you should get a popup asking if you want to `Reopen in container`. If you don't, you can use command palette (`cmd+shift+p` for mac) and type `dev container: Reopen in Container`.

If you have a Rails 7.2+ project created and you want to add devcontainers

```bash
rails devcontainer
```

Which creates the devcontainer files for you:

![output of running rails devcontainer. It figures out what libraries you use and creates the devcontainer folder and files for you.](https://res.cloudinary.com/jonathan-yeong/image/upload/v1725126469/unsigned_obsidian_uploads/pdf1bfzoym0gi0xme5pu.png)
## Initial thoughts
Here's what I loved:

- Modifying files in the container also modified the files on my local machine. And vice versa! It's a fast development workflow
- Git just works! It pulled my gitconfig from my machine, and I could push verified commits from the container.
- Since the app runs in a container, I don't need to worry about dependency management any more. If I need a new dependency, I can pull it into my docker image.
- It works out of the box with VSCode dev container extension.

Here's what I didn't like:

- You can't run `--skip-bundle` and generate a `.devcontainer` folder. Maybe someone can correct me if I'm wrong. I would like to not have to install anything locally if possible!
- The best dev experience is with VSCode. If you don't use that editor, your workflow would be more involved. You would want to use the [devcontainers/cli](https://github.com/devcontainers/cli).
- If you have an underpowered machine, dev containers might not be for you. If your container run time isn't beefy enough with RAM & CPU a-plenty, then your dev container might be sluggish.

[GitHub repo](https://github.com/jonathanyeong/rails-dev-containers) for reference.

## Troubleshooting
### Unable to connect to DB through a DB management tool
I'm seeing an issue with Rails 8 and the default devcontainer setup. When I try to connect to my devcontainer DB using [Sequel Ace](https://sequel-ace.com/) or [DBeaver](https://dbeaver.io/). Then I get a timeout error:

```
Unable to connect to host localhost, or the request timed out.
Be sure that the address is correct and that you have the necessary privileges, or try increasing the connection timeout (currently 10 seconds).
MySQL said: Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
```

To fix this issue remove `3306` from `forwardPorts` section in devcontainer.json. And add in `compose.yaml` add this line under the `mysql` block:

```
ports:
  - '3306:3306'
```

So the `mysql` block should now look like:

```
mysql:
  image: mysql/mysql-server:8.0
  restart: unless-stopped
  environment:
    MYSQL_ALLOW_EMPTY_PASSWORD: 'true'
	MYSQL_ROOT_HOST: "%"
  volumes:
  - mysql-data:/var/lib/mysql
  networks:
  - default
  ports:
	- '3306:3306'
```

Rebuild your devcontainer and you should be able to connect to your DB using sequel ace or DBeaver.

### Can't connect to local server through socket
**Update:** It looks like with the latest Rails release, these issues have been fixed.
When I first ran the dev container, the `bin/setup` step errored:

```
ActiveRecord::ConnectionNotEstablished: Can't connect to local server through socket '/tmp/mysql.sock'
```

The issue is that `config/database.yaml` is trying to read a `mysql.sock` file that doesn't exist in the container. The easiest fix is to connect to the DB through the network. Modify your `config/database.yaml` file with:

```yaml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  host: mysql
  port: 3306
```

After this change, I was able to get my dev container up and running. However, when I tried to do Git things like `git status`, I got an error:

```bash
fatal: detected dubious ownership in repository at '/workspaces/dev-containers-app'
To add an exception for this directory, call:
    git config --global --add safe.directory /workspaces/dev-containers-app

```

To resolve this issue, I updated the `devcontainer.json` file. Adding a `postStartCommand`.

```json
{
	... // Rest of config
	"postStartCommand": "git config --global --add safe.directory ${containerWorkspaceFolder}"
}
```
