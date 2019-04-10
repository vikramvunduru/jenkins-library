# mavenExecute

## Description

Executes a maven command inside a Docker container.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `defines` | no |  |  |
| `dockerImage` | no | `maven:3.5-jdk-7` |  |
| `dockerOptions` | no |  |  |
| `flags` | no |  |  |
| `globalSettingsFile` | no |  |  |
| `goals` | no |  |  |
| `logSuccessfulMavenTransfers` | no |  | `true`, `false` |
| `m2Path` | no |  |  |
| `pomPath` | no |  |  |
| `projectSettingsFile` | no |  |  |
| `script` | yes |  |  |

* `defines` - Additional properties.
* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerOptions` - Docker options to be set when starting the container (List or String).
* `flags` - Flags to provide when running mvn.
* `globalSettingsFile` - Path or url to the mvn settings file that should be used as global settings file.
* `goals` - Maven goals that should be executed.
* `logSuccessfulMavenTransfers` - Configures maven to log successful downloads. This is set to `false` by default to reduce the noise in build logs.
* `m2Path` - Path to the location of the local repository that should be used.
* `pomPath` - Path to the pom file that should be used.
* `projectSettingsFile` - Path or url to the mvn settings file that should be used as project settings file.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `defines` |  |  |
| `dockerImage` |  | X |
| `dockerOptions` |  |  |
| `flags` |  |  |
| `globalSettingsFile` |  | X |
| `goals` |  |  |
| `logSuccessfulMavenTransfers` |  |  |
| `m2Path` |  | X |
| `pomPath` |  | X |
| `projectSettingsFile` |  | X |
| `script` |  |  |

## Exceptions

None

## Example

```groovy
mavenExecute script: this, goals: 'clean install'
```
