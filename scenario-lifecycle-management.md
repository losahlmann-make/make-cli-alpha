# Scenario Lifecycle Management
A **Source-driven Strategy** with Git as external Source Version Control system, provides the most flexibility regarding Scenario version management and precise change tracking over time.

The key capability of the Make CLI is to *Pull* and *Push* Scenarios (packaged with all related Entity metadata) between Teams and Organizations with automatic *Dependency Injection*.

## Pull a Scenario and all related Entities as Package
To save a scenario with all its related entities as metadata files (Blueprints), use:

```bash
./make-cli scenario pull --config config.yml --package test-scenario/ --scenarioId 12345 
```

This is especially useful to commit these files into a Source Version Control system such as Git, or to package the scenario as a single artifact.

## Push a Scenario and all related Entities as Package
To create a scenario from a local blueprint package, use:

```bash
./make-cli scenario push --config config.yml --mapConnections --teamId 123 --package test-scenario/ --blueprint scenario.json --saveConfig config.yml
```

This command handles Connections:
- If the parameter `--mapConnections` is omitted, Make will pick up existing Connections in the target environment that exist for the applications used in the Scenario.
- If the parameter `--mapConnections` is set, the mappings defined in the config file (section `account`) are used to replace the Connections.


## Clone a Scenario and its dependent Entities into a Target Environment
To clone a Scenario from a source environment to a target environment, the following command can be used. The target Team can be in a different Organization (of the same Make instance).

```
./make-cli scenarios clone create --config config.yml --organizationId 1 --teamId 123  --scenarioId 12345 --name ClonedScenario
```

- The parameter `organizationId` defines the target Organization that needs to contain the Team given by `teamId`.
- The `scenarioId` is the Scenario to be cloned.

Entities can be remapped by providing the source and target entity ID pairs in the configuration file.
