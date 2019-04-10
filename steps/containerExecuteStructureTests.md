# containerExecuteStructureTests

## Description

In this step [Container Structure Tests](https://github.com/GoogleContainerTools/container-structure-test) are executed.

This testing framework allows you to execute different test types against a Docker container, for example:

* Command tests (only if a Docker Deamon is available)
* File existence tests
* File content tests
* Metadata test

## Prerequisites

Test configuration is available.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `containerCommand` | no | `/busybox/tail -f /dev/null` |  |
| `containerShell` | no | `/busybox/sh` |  |
| `dockerImage` | no | `ppiper/container-structure-test` |  |
| `dockerOptions` | no | `-u 0 --entrypoint=''` |  |
| `failOnError` | no | `true` | `true`, `false` |
| `pullImage` | no |  | `true`, `false` |
| `script` | yes |  |  |
| `stashContent` | no | `[tests]` |  |
| `testConfiguration` | no |  |  |
| `testDriver` | no |  |  |
| `testImage` | no |  |  |
| `testReportFilePath` | no | `cst-report.json` |  |
| `verbose` | no |  | `true`, `false` |

* `containerCommand` - Kubernetes only: Allows to specify start command for container created with dockerImage parameter to overwrite Piper default (`/usr/bin/tail -f /dev/null`).
* `containerShell` - Kubernetes only: Allows to specify the shell to be used for execution of commands.
* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerOptions` - Docker options to be set when starting the container (List or String).
* `failOnError` - Defines the behavior, in case tests fail.
* `pullImage` - Only relevant for testDriver 'docker'.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `stashContent` - Specific stashes that should be considered for the step execution.
* `testConfiguration` - Container structure test configuration in yml or json format. You can pass a pattern in order to execute multiple tests.
* `testDriver` - Container structure test driver to be used for testing, please see https://github.com/GoogleContainerTools/container-structure-test for details.
* `testImage` - Image to be tested
* `testReportFilePath` - Path and name of the test report which will be generated
* `verbose` - Print more detailed information into the log.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `containerCommand` |  | X |
| `containerShell` |  | X |
| `dockerImage` |  | X |
| `dockerOptions` |  | X |
| `failOnError` |  | X |
| `pullImage` |  | X |
| `script` |  |  |
| `stashContent` |  | X |
| `testConfiguration` |  | X |
| `testDriver` |  | X |
| `testImage` |  | X |
| `testReportFilePath` |  | X |
| `verbose` | X | X |

## Example

```
containerExecuteStructureTests(
  script: this,
  testConfiguration: 'config.yml',
  testImage: 'node:latest'
)
```
