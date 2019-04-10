# batsExecuteTests

## Description

This step executes tests using the [Bash Automated Testing System - bats-core](https://github.com/bats-core/bats-core)

## Prerequsites

You need to have a Bats test file. By default you would put this into directory `src/test` within your source code repository.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `dockerImage` | no | `node:8-stretch` |  |
| `dockerWorkspace` | no | `/home/node` |  |
| `envVars` | no |  |  |
| `failOnError` | no |  |  |
| `gitBranch` | no |  |  |
| `gitSshKeyCredentialsId` | no |  |  |
| `outputFormat` | no | `junit` | `tap` |
| `repository` | no | `https://github.com/bats-core/bats-core.git` |  |
| `script` | yes |  |  |
| `stashContent` | no | `[tests]` |  |
| `testPackage` | no | `piper-bats` |  |
| `testPath` | no | `src/test` |  |
| `testRepository` | no |  |  |

* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerWorkspace` - Kubernetes only: Specifies a dedicated user home directory for the container which will be passed as value for environment variable `HOME`.
* `envVars` - Defines the environment variables to pass to the test execution.
* `failOnError` - Defines the behavior, in case tests fail. For example, in case of `outputFormat: 'junit'` you should set it to `false`. Otherwise test results cannot be recorded using the `testsPublishhResults` step afterwards.
* `gitBranch` - Defines the branch where the tests are located, in case the tests are not located in the master branch.
* `gitSshKeyCredentialsId` - Defines the access credentials for protected repositories. Note: In case of using a protected repository, `testRepository` should include the ssh link to the repository.
* `outputFormat` - Defines the format of the test result output. `junit` would be the standard for automated build environments but you could use also the option `tap`.
* `repository` - Defines the version of **bats-core** to be used. By default we use the version from the master branch.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `stashContent` - Specific stashes that should be considered for the step execution.
* `testPackage` - For the transformation of the test result to xUnit format the node module **tap-xunit** is used. This parameter defines the name of the test package used in the xUnit result file.
* `testPath` - Defines either the directory which contains the test files (`*.bats`) or a single file. You can find further details in the [Bats-core documentation](https://github.com/bats-core/bats-core#usage).
* `testRepository` - Allows to load tests from another repository.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `dockerImage` |  | X |
| `dockerWorkspace` |  | X |
| `envVars` |  | X |
| `failOnError` |  | X |
| `gitBranch` |  | X |
| `gitSshKeyCredentialsId` |  | X |
| `outputFormat` |  | X |
| `repository` |  | X |
| `script` |  |  |
| `stashContent` |  | X |
| `testPackage` |  | X |
| `testPath` |  | X |
| `testRepository` |  | X |

## Example

```groovy
batsExecuteTests script:this
testsPublishResults junit: [pattern: '**/Test-*.xml', archive: true]
```

    With `envVars` it is possible to pass either fixed values but also templates using [`commonPipelineEnvironment`](commonPipelineEnvironment.md).

    Example:

    ```yaml
    batsExecuteTests script: this, envVars = [
      FIX_VALUE: 'my fixed value',
      CONTAINER_NAME: '${commonPipelineEnvironment.configuration.steps.executeBatsTests.dockerContainerName}',
      IMAGE_NAME: '${return commonPipelineEnvironment.getDockerImageNameAndTag()}'
    ]
    ```

    This means within the test one could refer to environment variables by calling e.g.
    `run docker run --rm -i --name \$CONTAINER_NAME --entrypoint /bin/bash \$IMAGE_NAME echo "Test"`
