# The Make CLI
The **Make Command Line Interface (CLI)** is a tool that bundles multiple actions into a single command. These actions can be executed manually or as part of an automated workflow, such as from CI/CD pipelines.

As such it fits into developers' workflows that are used to work with other command line tools, in particular *Git*.

A **Source-driven Strategy** with Git as external Source Version Control system, provides the most flexibility regarding scenario lifecycle management and precise change tracking over time.

> [!TIP]
> The key capability of the Make CLI is to *Pull* and *Push* Scenarios (packaged with all related dependency metadata) between Teams and Organizations with automatic *Dependency Injection*.

## Installation
Please follow these steps to install the Make CLI:
1. Make sure you have the [Node.js runtime installed](https://nodejs.org/en/download).
2. [Download](https://github.com/losahlmann-make/make-cli-alpha/releases/latest) the Make CLI executable.
3. Make the file `make-cli` executable: In the command line, change into the downloaded and extracted directory and use:
  ```bash
chmod 755 make-cli
```
4. Run the Make CLI to test:
  ```bash
./make-cli --help
```
5. You should see a list of all available commands as output.

> [!WARNING]
> The Make CLI is currently a **proof-of-concept** and not officially supported by Make. **Please use at your own risk!**

### Create a Make API Token
The Make CLI bundles functionality provided by the Make API. For that reason it needs to be authorised to access your Make environment through a Make API Token.

Please [follow these steps](https://www.make.com/en/help/apps/process-management/make#connectmake) to create a token for your user.

## Using a Configuration File
The Make CLI can take a configuration file via the parameter `--config`. Alternatively, all parameters can be provided through the command line directly.

For security reasons, it is recommended to provide the Make API Token through a config file to the Make CLI and to set the file permissions correctly.

The configuration file is a YAML file of the following structure:
    
```YAML
domain: eu1.make.com
token: xxxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxxxx
account:
  eu1_123: 456
datastore:
  eu2_432: 987
hook:
  us1_101: 202
```

- The `domain` is the Make instance on which your (target) Organization is hosted.
- The `token` is the Make API token for your user.
- The sections `account`, `datastore`, `hook` define the IDs of entities in the source environment with their corresponding entity ID in the target environment. These sections are usually auto-populated by the CLI.

## Usage
[Scenario Lifecycle Management](scenario-lifecycle-management.md)
