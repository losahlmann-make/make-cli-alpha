# The Make CLI
The **Make Command Line Interface (CLI)** is a tool that bundles multiple actions into a single command that can be executed manually or as part of an automated workflow, such as from CI/CD pipelines.

As such it fits into developers' workflows that are used to work with other command line tools, in particular *Git*.

A **Package-driven Approach** with Git as external Source Version Control system, provides the most flexibility regarding Scenario version management and precise change tracking over time.

The key capability of the Make CLI is to *Pull* and *Push* Scenarios (packaged with all related Entity metadata) between Teams and Organizations with automatic *Dependency Injection*.

## Install the Make CLI
Please follow these steps to install the Make CLI:
- Make sure you have the [Node.js runtime installed](https://nodejs.org/en/download).
- [Download](https://github.com/losahlmann-make/make-cli-alpha/releases/latest) the Make CLI source.
- Make the file executable: In the command line, change into the downloaded and extracted directory and use:
  - `chmod 755 make-cli`
- Run the Make CLI to test:
  - `./make-cli`
- You should see a list of all available commands as output.

### Create a Make API Token
The Make CLI bundles functionality provided by the Make API. For that reason it needs to be authorised to access your Make environment through a Make API Token.

Please [follow these steps](https://www.make.com/en/help/apps/process-management/make#connectmake) to create a token for your user.

### Using a Configuration File
The Make CLI can take a configuration file via the parameter `--config` instead of having all parameters provided through the command line directly.

For security reasons, it is recommended to provide the Make API Token through a config file to the Make CLI.

The configuration file is a YAML file of the following structure.
    
```YAML
domain: eu1.make.com
token: xxxxxxxx-xxxx-xxx-xxxx-xxxxxxxxxxxx
account:
  '123': 456
datastore:
  '432': 987
hook:
  '101': 202
```

The `domain` is the Make environment on which your target Organization is hosted.
The `token` is the Make API token.
The sections `account`, `datastore`, `hook` define the IDs of entities in the source environment with their corresponding Entity ID in the target environment.

## Usage

### Clone a Scenario and its dependent Entities into a Target Environment
To clone a Scenario from a source environment to a target environment, the following command can be used. The target Team can be in a different Organization (of the same Make instance).

```
./make-cli scenarios clone create --config config.yml --organizationId 1 --teamId 123  --scenarioId 12345 --name ClonedScenario
```

- The parameter `organizationId` defines the target Organization that needs to contain the Team given by `teamId`.
- The `scenarioId` is the Scenario to be cloned.

Entities can be remapped by providing the source and target entity ID pairs in the configuration file.

### Pull a Scenario and all related Entities as Package
To save a Scenario with all its related Entities as metadata files (Blueprints), use:

```
./make-cli scenarios pull --config config.yml --scenarioId 12345 --organizationId 1 --teamId 123 --output test-scenario/
```

This is especially useful to commit these files into a Source Version Control system such as Git, or to package the Scenario as a single artifact.

### Push a Scenario and all related Entities as Package
To create a Scenario from a local Blueprint file, use:

```
./make-cli scenarios push --config config.yml --mapConnections --organizationId 1 --teamId 123 --package test-scenario/
```

This command handles Connections:
- If the parameter `--mapConnections` is omitted, Make will pick up existing Connections in the target environment that exist for the applications used in the Scenario.
- If the parameter `--mapConnections` is set, the mappings defined in the config file (section `account`) are used to replace the Connections.
