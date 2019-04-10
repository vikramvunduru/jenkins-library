# karmaExecuteTests

## Description

In this step the ([Karma test runner](http://karma-runner.github.io)) is executed.

The step is using the `seleniumExecuteTest` step to spin up two containers in a Docker network:

* a Selenium/Chrome container (`selenium/standalone-chrome`)
* a NodeJS container (`node:8-stretch`)

In the Docker network, the containers can be referenced by the values provided in `dockerName` and `sidecarName`, the default values are `karma` and `selenium`. These values must be used in the `hostname` properties of the test configuration ([Karma](https://karma-runner.github.io/1.0/config/configuration-file.html) and [WebDriver](https://github.com/karma-runner/karma-webdriver-launcher#usage)).

!!! note
    In a Kubernetes environment, the containers both need to be referenced with `localhost`.

## Prerequisites

* **running Karma tests** - have a NPM module with running tests executed with Karma
* **configured WebDriver** - have the [`karma-webdriver-launcher`](https://github.com/karma-runner/karma-webdriver-launcher) package installed and a custom, WebDriver-based browser configured in Karma

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `containerPortMappings` | no | `[node:8-stretch:[[containerPort:9876, hostPort:9876]]]` |  |
| `dockerEnvVars` | no | `[NO_PROXY:localhost,selenium,$NO_PROXY, no_proxy:localhost,selenium,$no_proxy]` |  |
| `dockerImage` | no | `node:8-stretch` |  |
| `dockerName` | no | `karma` |  |
| `dockerWorkspace` | no | `/home/node` |  |
| `failOnError` | no |  | `true`, `false` |
| `installCommand` | no | `npm install --quiet` |  |
| `modules` | no | `[.]` |  |
| `runCommand` | no | `npm run karma` |  |
| `script` | yes |  |  |
| `sidecarEnvVars` | no | `[NO_PROXY:localhost,karma,$NO_PROXY, no_proxy:localhost,karma,$no_proxy]` |  |
| `sidecarImage` | no |  |  |
| `sidecarName` | no |  |  |
| `sidecarVolumeBind` | no |  |  |
| `stashContent` | no | `[buildDescriptor, tests]` |  |

* `containerPortMappings` - Map which defines per docker image the port mappings, e.g. `containerPortMappings: ['selenium/standalone-chrome': [[name: 'selPort', containerPort: 4444, hostPort: 4444]]]`.
* `dockerEnvVars` - A map of environment variables to set in the container, e.g. [http_proxy:'proxy:8080'].
* `dockerImage` - The name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerName` - Kubernetes only: Name of the container launching `dockerImage`. SideCar only: Name of the container in local network.
* `dockerWorkspace` - Kubernetes only: Specifies a dedicated user home directory for the container which will be passed as value for environment variable `HOME`.
* `failOnError` - With `failOnError` the behavior in case tests fail can be defined.
* `installCommand` - The command that is executed to install the test tool.
* `modules` - Define the paths of the modules to execute tests on.
* `runCommand` - The command that is executed to start the tests.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `sidecarEnvVars` - A map of environment variables to set in the sidecar container, similar to `dockerEnvVars`.
* `sidecarImage` - The name of the docker image of the sidecar container. If empty, no sidecar container is started.
* `sidecarName` - as `dockerName` for the sidecar container
* `sidecarVolumeBind` - Volumes that should be mounted into the sidecar container.
* `stashContent` - If specific stashes should be considered for the tests, their names need to be passed via the parameter `stashContent`.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `containerPortMappings` | X | X |
| `dockerEnvVars` | X | X |
| `dockerImage` | X | X |
| `dockerName` | X | X |
| `dockerWorkspace` | X | X |
| `failOnError` | X | X |
| `installCommand` | X | X |
| `modules` | X | X |
| `runCommand` | X | X |
| `script` |  |  |
| `sidecarEnvVars` | X | X |
| `sidecarImage` | X | X |
| `sidecarName` | X | X |
| `sidecarVolumeBind` | X | X |
| `stashContent` | X | X |

## Side effects

Step uses `seleniumExecuteTest` & `dockerExecute` inside.

## Exceptions

none

## Example

```groovy
karmaExecuteTests script: this, modules: ['./shoppinglist', './catalog']
```
