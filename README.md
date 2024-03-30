# Terraform Beginner Bootcamp 2023

## Semantic Versioning  :mage:

This project is goint to utilize semantic versioning for it's tagging.

[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with Terraform CLI changes
The Terraform CLI installation have changed due to gpg keyring changes. Refer to the latest install CLI instructions via Terraform Documentation and change the script for installing.

[Install the Terraform CLI](https://developer.hashicorp.com/terraform/install)

### Considerations for Linux Distribution
This project is built against Ubuntu.
Please consider checking your Linux Distribution and change accordingly to distributions needs.

[How to check OS Version in Linux](https://linuxize.com/post/how-to-check-linux-version/#:~:text=lsb_release%20command%20The%20lsb_release%20utility%20displays%20LSB%20%28Linux,Linux%20distributions%20that%20have%20the%20lsb-release%20package%20installed%3A
)

Example of how to check OS Version:

```
$ cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.4 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.4 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Refactoring into Bash Scripts

While fixing the Terrafirn CLI gpg deprecation issues, we notice that bash scripts steps were a considerable ammoun more code. So we decided to create a bash script to install the Terraform CLI.

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml)) tidy.
- Allows us an easier to debug and execute manually Terraform CLI Install
- This will allow better portability for other projects that need to install Terraform CLI.
- The first line is a custom line for removing the keyring if there is and older one for install Terraform more smootly.

#### Shebang Considerations

A Shebang (pronounced Sha-bang) tells the bash script what program will interpret the script. eg. `#!/bin/bash`

ChatGPT recomends this format for bash `#!/usr/bin/env bash`

- for portability for different OS distributions.
- will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_%28Unix%29

#### Executions Considerations: 
When Executing the bash script we can use `./` shorthand notation to execute the bash script.

eg. `./bin/install_terraform_cli`

If we are using a script in gitpod.yml we need to point the script to a program to interpret it.

eg. `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

In order to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```
Alternatively:

```sh
chmod 744 ./bin/install_terraform_cli
```

https://linuxize.com/post/understanding-linux-file-permissions/

### Gitpod Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerunt if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks

### Working Env Vars

We can list out all Environment Variables (Env Vargs) using the `env` command

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world'`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command

``` sh
HELLO='world' ./bin/print_message
```

Within a bash script we can set env without writing export eg. 

``` sh
HELLO='world'

echo $HELLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open upo new bash terminals in VSCode it will not be aware of env vars that you have set in another window.

If you want to Env Var to persist accross all future bash terminals that are open, you need to set env vars in your bash profile. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod
We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspace launched will set the env vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only containg non-sensitive env vars.