# Scenario Lifecycle Management
A **Source-driven Strategy** with Git as external *Repository* and Source Version Control system, provides the most flexibility regarding scenario lifecycle management and precise change tracking over time.

> [!IMPORTANT]
> The key capability of the Make CLI is to *Pull* and *Push* Scenarios (packaged with all related Entity metadata) between Teams and Organizations with automatic *Dependency Injection*.

## Pull a Scenario and all related Entities
To save a scenario with all its related entities as metadata files in a local repository, use:

```bash
./make-cli scenario pull --config config.yml --repo repo/ --scenarioId 12345
```

This is especially useful to track scenarios in a Source Version Control system such as Git, or to package the scenario and its related entities as a single artifact.

## Push a Scenario and all related Entities
To create a new scenario and related entities in a target Team from a local repository, use:

```bash
./make-cli scenario push --config config.yml --saveConfig config.yml --teamId 112890 --repo repo/ --blueprint test_scenario.json
```

This command handles Connections:
- If the parameter `--defaultConnections` is omitted, the mappings defined in the config file (section `account`) are used to replace the Connection
- If the parameter `--defaultConnections` is set, Make will pick up existing Connections in the target environment that exist for the applications used in the Scenario.

## Clone a Scenario and its dependent Entities into a Target Environment
To clone a Scenario from a source environment to a target environment, the following command can be used. The target Team can be in a different Organization (of the same Make instance).

```
./make-cli scenarios clone create --config config.yml --organizationId 1 --teamId 123  --scenarioId 12345 --name ClonedScenario
```

- The parameter `organizationId` defines the target Organization that needs to contain the Team given by `teamId`.
- The `scenarioId` is the Scenario to be cloned.

Entities can be remapped by providing the source and target entity ID pairs in the configuration file.
