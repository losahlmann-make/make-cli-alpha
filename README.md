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
User specific configuration can be stored in a configuration file. By default, the configuration is read from the file `$HOME/.config/make-cli/config.yml`, if not specified differently by (in order of precedence):
1. The command line option `--config`, pointing to the file to use.
2. The environment variable `MAKE_CONFIG` pointing to a configuration file.
3. The environment variable `XDG_CONFIG_HOME` is pointing to a directory different to `$HOME/.config`. Expecting a file `make-cli/config.yml` in this configuration directory.

The Make CLI can take a configuration file via the parameter `--config`. Alternatively, all parameters can be provided through the command line directly.

For security reasons, it is recommended to provide the Make API Token through a config file to the Make CLI and to set the file permissions correctly. Don't track the config file containing the secret token in source version control.

The configuration file is a YAML file of the following structure:
    
```YAML
token: xxxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxxxx
domain: eu1.make.com
```

- The `token` is the Make API token for your user.
- The `domain` is the Make instance on which your (target) Organization is hosted.

## Environments
The command `scenario push` uses an environment file.
In the `environments` file, a section is defined for each target environment. It can have any name, such as `dev`.
Each section needs the target Team Id and the domain for the Make instance where the target Organization is located. (If no domain is set, the default domain from the configuration file is used).

```YAML
dev:
  domain: eu1.make.com
  team: 2
  account:
    eu1_123: 456
  datastore:
    eu2_432: 987
  hook:
    us1_ent_101: 202

prod:
  domain: eu1.make.com
  team: 3
```

- The sub-sections `account`, `datastore`, `hook` ect. define the IDs of entities in the source environment with their corresponding entity ID in the target environment. These sections are usually auto-populated by the CLI.

By default, the file `env.yml` in the `--repo` directory is used. Is can be set explicitely using the option `--env-file`.

## Usage
[Scenario Lifecycle Management](scenario-lifecycle-management.md)

## Development

### Build from Source
1. Clone the repository (With git subtrees)
2. Install dependencies `npm install`
3. Install code generation dependencies `npm run codegen:install`
4. Auto-generate code for CLI commands based on Make API endpoints: `npm run codegen`
5. Then build using `npm run build` (installs Elm packages).
6. Use `npm run bundle` to combine the compiled Elm code and the JS platform code into a single file. This file is minimized.

Bundle flags for pure functions and external Node modules does not reduce output size.
No pure functions: 217.549 bytes
With pure functions: no difference

### Execute
Execute CLI via ```./make-cli```.

### Test
Needs [`yq`](https://github.com/mikefarah/yq) installed.
There are predefined commands to test the Elm code with `npm run test:elm` and to run the end-to-end test of the CLI commands with `npm run test:cli`.

### Upgrade Dependencies
### NPM Packages
`npm run upgrade:npm`
### Main Elm Packages
`npm run upgrade:elm`
### Elm Codegen Packages
### Elm Review Packages

### Vendored Elm Packages
Git Subtrees
- `elm-cli-options-parser`: `git subtree pull --prefix packages/elm-cli-options-parser git@github.com:losahlmann-make/elm-cli-options-parser.git master --squash`

### Update Make API Definition
To pull the latest version of the OpenAPI definition for the Make API, use `npm run api:json:pull`.
The API definition needs some manual fixes to be conform with the OpenAPI and JSON Schema standards, so that the code-gen script can consume it.
