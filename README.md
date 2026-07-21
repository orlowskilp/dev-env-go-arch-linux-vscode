# Go development environment for VSCode

[![MIT License](https://img.shields.io/badge/license-MIT-green)](/LICENSE)

This template provides a Go development environment for Visual Studio Code.
The development container is based on the Arch Linux image.

## Getting started

### Prerequisites

There's a couple of tools you will need

#### Simple - Docker + VS Code

To keep things simple I recommend [Docker](https://docs.docker.com/get-started/). Docker natively integrates with [VS Code](https://code.visualstudio.com/). This template is designed with this set up in mind. You can either run the devcontainer on the machine you're developing on or run a devcontainer on a separate machine and connect to it over SSH (like I do).

#### Advanced - K8s/K3s + DevPod

If you want to stay IDE agnostic and/or if you want to plug in your devcontainer into your dev environment, you can use [Kubernetes](https://kubernetes.io/) (or [K3s](https://k3s.io/)) and [DevPod](https://devpod.sh/).
This is way more complex and will present challenges, especially if you want to sync code changes with pod host storage real time.
This will allow you directly propagate changes to your local development environment. Additionally, you can use whichever IDE you like, as you connect to your devcontainer pod over SSH.

**Note:** This template won't work right off the bat. It will require some changes to [devcontainer.json](.devcontainer/devcontainer.json).

### Initializing the Go module

The goal was to make a clean separation between the codebase directory (i.e. the workspace) and the directory storing Go artifacts and configuration. The workspace is bind-mounted to a host directory, hence persistent; the Go artifacts are stored in container's `$HOME` directory, which is ephemeral.

Create your repository from the template, clone it locally and ppen the devcontainer (here I assume VS Code).

Since the workspace directory (`/workspace`) sits outside the `GOPATH`, so you must supply a full module path:

```shell
go mod init github.com/your-username/your-module
```

### Additional tools included

#### Formatting with `dprint`

This project uses [dprint](https://dprint.dev/) for deterministic formatting of Markdown, TOML, JSON, YAML, and Dockerfiles. Run `dprint fmt` before committing (or better yet, add a pre-commit git hook running `dprint check`). The relevant files are listed in `dprint.json`.

#### Environment variables with `direnv`

This project uses [direnv](https://direnv.net/) to load environment variables from a `.envrc` file whenever you enter the workspace directory. To get started, add your variables to `.envrc`:

```shell
export AWS_ACCESS_KEY_ID="..."
export AWS_SECRET_ACCESS_KEY="..."
```

Save the file and approve new variables with:

```shell
direnv allow
```

The variables are automatically unset when you leave the directory.

## License

This project is licensed under the MIT License - see the [LICENSE](/LICENSE) file for details.

---

Copyright (c) Lukasz P. Orlowski <lukasz@orlowski.io>
