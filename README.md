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

## Authorization
The Make CLI bundles functionality provided by the Make API. For that reason it needs to be authorized to access your Make environment through a Make API Token [created](https://www.make.com/en/help/apps/process-management/make#connectmake) for your user.

Make sure to handle your API token in a way which follows your security requirements and don't expose the token unintentionally.

The token can be passed to the CLI in multiple different ways, in the following order of precedence:

### Pipe in via `STDIN`
TODO: to be implemented

### `MAKE_TOKEN` Environment Variable
If this environvariable is set, the Make CLI will read the token from it. Usually this option is used with a secret manager, like 1Password or Terrafrom Vault, to not expose the token:
```bash
export MAKE_TOKEN="op://Dev/Make/password"
op run -- ./make-cli
```

### Configuration File
If a Configuration file is used (see next section), the token can be saved there under the key `token`.

## Configuration
The Make CLI can be configured in multiple ways, with the following order of precedence of options supporting multiple configuration ways:
1. Command line options/flags
2. Environment variables (if available for the configuration option)
3. Configuration file

### Using a Configuration File
By default, the configuration is read from the file `$HOME/.config/make-cli/config.yml`, if not specified differently by (in order of precedence):
1. The command line option `--config`, pointing to the file to use.
2. The environment variable `MAKE_CONFIG` pointing to a configuration file.
3. The environment variable `XDG_CONFIG_HOME` is pointing to a directory different to `$HOME/.config`. Expecting a file `make-cli/config.yml` in this configuration directory.

The Make CLI can take a configuration file via the parameter `--config`. Alternatively, all parameters can be provided through the command line directly.

For security reasons, it is recommended to provide the Make API Token through a config file to the Make CLI and to set the file permissions correctly.

The configuration file is a YAML file of the following structure:
    
```YAML
domain: eu1.make.com
token: xxxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxxxx
environments:
  eu1_234:
    account:
      eu1_123: 456
    datastore:
      eu2_432: 987
    hook:
      us1_ent_101: 202
```

- The `domain` is the Make instance on which your (target) Organization is hosted.
- The `token` is the Make API token for your user.
- In the `environments` block, a section is defined for each target environment, which is named after the target zone and the target team ID in this zone.
- The sub-sections `account`, `datastore`, `hook` define the IDs of entities in the source environment with their corresponding entity ID in the target environment. These sections are usually auto-populated by the CLI.

## Usage
[Scenario Lifecycle Management](scenario-lifecycle-management.md)
