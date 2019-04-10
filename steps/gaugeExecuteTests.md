# gaugeExecuteTests

## Description

In this step Gauge ([getgauge.io](http:getgauge.io)) acceptance tests are executed.
Using Gauge it will be possible to have a three-tier test layout:

* Acceptance Criteria
* Test implemenation layer
* Application driver layer

This layout is propagated by Jez Humble and Dave Farley in their book "Continuous Delivery" as a way to create maintainable acceptance test suites (see "Continuous Delivery", p. 190ff).

Using Gauge it is possible to write test specifications in [Markdown syntax](http://daringfireball.net/projects/markdown/syntax) and therefore allow e.g. product owners to write the relevant acceptance test specifications. At the same time it allows the developer to implement the steps described in the specification in her development environment.

You can use the [sample projects](https://github.com/getgauge/gauge-mvn-archetypes) of Gauge.

!!! note "Make sure to run against a Selenium Hub configuration"
    In the test example of _gauge-archetype-selenium_ please make sure to allow it to run against a Selenium hub:

    Please extend DriverFactory.java for example in following way:

    ``` java
    String hubUrl = System.getenv("HUB_URL");
    //when running on a Docker deamon (and not using Kubernetes plugin), Docker images will be linked
    //in this case hubUrl will be http://selenium:4444/wd/hub due to the linking of the containers
    hubUrl = (hubUrl == null) ? "http://localhost:4444/wd/hub" : hubUrl;
    Capabilities chromeCapabilities = DesiredCapabilities.chrome();
    System.out.println("Running on Selenium Hub: " + hubUrl);
    return new RemoteWebDriver(new URL(hubUrl), chromeCapabilities);
    ```

## Prerequsites

none

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `buildTool` | no | `maven` |  |
| `dockerEnvVars` | no | `[HUB:TRUE, HUB_URL:http://localhost:4444/wd/hub]` |  |
| `dockerImage` | no |  |  |
| `dockerName` | no |  |  |
| `dockerWorkspace` | no |  |  |
| `failOnError` | no |  | true, false |
| `gitBranch` | no |  |  |
| `gitSshKeyCredentialsId` | no |  |  |
| `installCommand` | no | `curl -SsL https://downloads.gauge.org/stable | sh -s -- --location=$HOME/bin/gauge` |  |
| `languageRunner` | no |  |  |
| `runCommand` | no |  |  |
| `script` | yes |  |  |
| `stashContent` | no | `[buildDescriptor, tests]` |  |
| `testOptions` | no |  |  |
| `testRepository` | no |  |  |
| `testServerUrl` | no |  |  |

* `buildTool` - Defines the build tool to be used for the test execution.
* `dockerEnvVars` - Environment variables to set in the container, e.g. [http_proxy: 'proxy:8080'].
* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerName` - Kubernetes only: Name of the container launching `dockerImage`. SideCar only: Name of the container in local network.
* `dockerWorkspace` - Kubernetes only: Specifies a dedicated user home directory for the container which will be passed as value for environment variable `HOME`.
* `failOnError` - Defines the behavior in case tests fail. When this is set to `true` test results cannot be recorded using the `publishTestResults` step afterwards.
* `gitBranch` - Defines the branch containing the tests, in case the test implementation is stored in a different repository and a different branch than master.
* `gitSshKeyCredentialsId` - Defines the credentials for the repository containing the tests, in case the test implementation is stored in a different and protected repository than the code itself. For protected repositories the `testRepository` needs to contain the ssh git url.
* `installCommand` - Defines the command for installing Gauge. In case the `dockerImage` already contains Gauge it can be set to empty: ``.
* `languageRunner` - Defines the Gauge language runner to be used.
* `runCommand` - Defines the command which is used for executing Gauge.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `stashContent` - Defines if specific stashes should be considered for the tests.
* `testOptions` - Allows to set specific options for the Gauge execution. Details can be found for example [in the Gauge Maven plugin documentation](https://github.com/getgauge/gauge-maven-plugin#executing-specs)
* `testRepository` - Defines the repository containing the tests, in case the test implementation is stored in a different repository than the code itself.
* `testServerUrl` - It is passed as environment variable `TARGET_SERVER_URL` to the test execution. Tests running against the system should read the host information from this environment variable in order to be infrastructure agnostic.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `buildTool` |  | X |
| `dockerEnvVars` |  | X |
| `dockerImage` |  | X |
| `dockerName` |  | X |
| `dockerWorkspace` |  | X |
| `failOnError` |  | X |
| `gitBranch` |  | X |
| `gitSshKeyCredentialsId` |  | X |
| `installCommand` |  | X |
| `languageRunner` |  | X |
| `runCommand` |  | X |
| `script` |  |  |
| `stashContent` |  | X |
| `testOptions` |  | X |
| `testRepository` |  | X |
| `testServerUrl` |  | X |

We recommend to define values of step parameters via [config.yml file](../configuration.md).

## Example

Pipeline step:

```groovy
gaugeExecuteTests script: this, testServerUrl: 'http://test.url'
```
