# seleniumExecuteTests

## Description

Enables UI test execution with Selenium in a sidecar container.

The step executes a closure (see example below) connecting to a sidecar container with a Selenium Server.

When executing in a

* local Docker environment, please make sure to set Selenium host to **`selenium`** in your tests.
* Kubernetes environment, plese make sure to set Seleniums host to **`localhost`** in your tests.

!!! note "Proxy Environments"
    If work in an environment containing a proxy, please make sure that `localhost`/`selenium` is added to your proxy exclusion list, e.g. via environment variable `NO_PROXY` & `no_proxy`. You can pass those via parameters `dockerEnvVars` and `sidecarEnvVars` directly to the containers if required.

## Prerequisites

none

## Example

```groovy
seleniumExecuteTests (script: this) {
    git url: 'https://github.wdf.sap.corp/xxxxx/WebDriverIOTest.git'
    sh '''npm install
        node index.js'''
}
```

### Example test using WebdriverIO

Example based on http://webdriver.io/guide/getstarted/modes.html and http://webdriver.io/guide.html

#### Configuration for Local Docker Environment

```js
var webdriverio = require('webdriverio');
var options = {
    host: 'selenium',
    port: 4444,
    desiredCapabilities: {
        browserName: 'chrome'
    }
};
```

#### Configuration for Kubernetes Environment

```js
var webdriverio = require('webdriverio');
var options = {
    host: 'localhost',
    port: 4444,
    desiredCapabilities: {
        browserName: 'chrome'
    }
};
```

#### Test Code (index.js)

```js
// ToDo: add configuration from above

webdriverio
    .remote(options)
    .init()
    .url('http://www.google.com')
    .getTitle().then(function(title) {
        console.log('Title was: ' + title);
    })
    .end()
    .catch(function(err) {
        console.log(err);
    });
```

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `buildTool` | no | `npm` | `'maven'`, `'npm'` |
| `containerPortMappings` | no | `[selenium/standalone-chrome:[[containerPort:4444, hostPort:4444]]]` |  |
| `dockerEnvVars` | no |  |  |
| `dockerImage` | no |  |  |
| `dockerName` | no |  |  |
| `dockerWorkspace` | no |  |  |
| `failOnError` | no | `true` | `true`, `false` |
| `gitBranch` | no |  |  |
| `gitSshKeyCredentialsId` | no |  | Jenkins credentials id |
| `script` | yes |  |  |
| `sidecarEnvVars` | no |  |  |
| `sidecarImage` | no | `selenium/standalone-chrome` |  |
| `sidecarName` | no | `selenium` |  |
| `sidecarVolumeBind` | no | `[/dev/shm:/dev/shm]` |  |
| `stashContent` | no | `[tests]` |  |
| `testRepository` | no |  |  |

* `buildTool` - Defines the tool which is used for executing the tests
* `containerPortMappings` - Map which defines per docker image the port mappings, e.g. `containerPortMappings: ['selenium/standalone-chrome': [[name: 'selPort', containerPort: 4444, hostPort: 4444]]]`.
* `dockerEnvVars` - Environment variables to set in the container, e.g. [http_proxy: 'proxy:8080'].
* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerName` - Kubernetes only: Name of the container launching `dockerImage`. SideCar only: Name of the container in local network.
* `dockerWorkspace` - Kubernetes only: Specifies a dedicated user home directory for the container which will be passed as value for environment variable `HOME`.
* `failOnError` - With `failOnError` the behavior in case tests fail can be defined.
* `gitBranch` - Only if `testRepository` is provided: Branch of testRepository, defaults to master.
* `gitSshKeyCredentialsId` - Only if `testRepository` is provided: Credentials for a protected testRepository
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `sidecarEnvVars` - as `dockerEnvVars` for the sidecar container
* `sidecarImage` - as `dockerImage` for the sidecar container
* `sidecarName` - as `dockerName` for the sidecar container
* `sidecarVolumeBind` - as `dockerVolumeBind` for the sidecar container
* `stashContent` - Specific stashes that should be considered for the step execution.
* `testRepository` - Define an additional repository where the test implementation is located. For protected repositories the `testRepository` needs to contain the ssh git url.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `buildTool` | X | X |
| `containerPortMappings` | X | X |
| `dockerEnvVars` | X | X |
| `dockerImage` | X | X |
| `dockerName` | X | X |
| `dockerWorkspace` | X | X |
| `failOnError` | X | X |
| `gitBranch` | X | X |
| `gitSshKeyCredentialsId` | X | X |
| `script` |  |  |
| `sidecarEnvVars` | X | X |
| `sidecarImage` | X | X |
| `sidecarName` | X | X |
| `sidecarVolumeBind` | X | X |
| `stashContent` | X | X |
| `testRepository` | X | X |

## Side effects

none

## Exceptions

none
