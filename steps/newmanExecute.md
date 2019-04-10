# newmanExecute

## Description

This script executes [Postman](https://www.getpostman.com) tests from a collection via the [Newman](https://www.getpostman.com/docs/v6/postman/collection_runs/command_line_integration_with_newman) command line tool.

## Prerequisites

* prepared Postman with a test collection

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `dockerImage` | no | `node:8-stretch` |  |
| `failOnError` | no | `true` | `true`, `false` |
| `gitBranch` | no |  |  |
| `gitSshKeyCredentialsId` | no |  | Jenkins credentials id |
| `newmanCollection` | no | `**/*.postman_collection.json` |  |
| `newmanEnvironment` | no |  |  |
| `newmanGlobals` | no |  |  |
| `newmanInstallCommand` | no | `npm install newman newman-reporter-html --global --quiet` |  |
| `newmanRunCommand` | no | `run '${config.newmanCollection}' --environment '${config.newmanEnvironment}' --globals '${config.newmanGlobals}' --reporters junit,html --reporter-junit-export 'target/newman/TEST-${collectionDisplayName}.xml' --reporter-html-export 'target/newman/TEST-${collectionDisplayName}.html'` |  |
| `script` | yes |  |  |
| `stashContent` | no | `[tests]` |  |
| `testRepository` | no |  |  |

* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `failOnError` - Defines the behavior, in case tests fail.
* `gitBranch` - Only if `testRepository` is provided: Branch of testRepository, defaults to master.
* `gitSshKeyCredentialsId` - Only if `testRepository` is provided: Credentials for a protected testRepository
* `newmanCollection` - The test collection that should be executed. This could also be a file pattern.
* `newmanEnvironment` - Specify an environment file path or URL. Environments provide a set of variables that one can use within collections. see also [Newman docs](https://github.com/postmanlabs/newman#newman-run-collection-file-source-options)
* `newmanGlobals` - Specify the file path or URL for global variables. Global variables are similar to environment variables but have a lower precedence and can be overridden by environment variables having the same name. see also [Newman docs](https://github.com/postmanlabs/newman#newman-run-collection-file-source-options)
* `newmanInstallCommand` - The shell command that will be executed inside the docker container to install Newman.
* `newmanRunCommand` - The newman command that will be executed inside the docker container.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `stashContent` - If specific stashes should be considered for the tests, you can pass this via this parameter.
* `testRepository` - Define an additional repository where the test implementation is located. For protected repositories the `testRepository` needs to contain the ssh git url.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `dockerImage` |  | X |
| `failOnError` |  | X |
| `gitBranch` |  | X |
| `gitSshKeyCredentialsId` |  | X |
| `newmanCollection` |  | X |
| `newmanEnvironment` |  | X |
| `newmanGlobals` |  | X |
| `newmanInstallCommand` |  | X |
| `newmanRunCommand` |  | X |
| `script` |  |  |
| `stashContent` |  | X |
| `testRepository` |  | X |

## Side effects

Step uses `dockerExecute` inside.

## Exceptions

none

## Example

Pipeline step:

```groovy
newmanExecute script: this
```

This step should be used in combination with `testsPublishResults`:

```groovy
newmanExecute script: this, failOnError: false
testsPublishResults script: this, junit: [pattern: '**/newman/TEST-*.xml']
```
