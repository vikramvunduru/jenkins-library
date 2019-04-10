# pipelineExecute

## Description

Loads and executes a pipeline from another git repository.
The idea is to set up a pipeline job in Jenkins that loads a minimal pipeline, which
in turn loads the shared library and then uses this step to load the actual pipeline.

A centrally maintained pipeline script (Jenkinsfile) can be re-used by
several projects using `pipelineExecute` as outlined in the example below.

## Prerequisites

none

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `branch` | no | `master` |  |
| `credentialsId` | no |  |  |
| `path` | no | `Jenkinsfile` |  |
| `repoUrl` | yes |  |  |
| `script` | yes |  |  |

* `branch` - The branch of the git repository from which the pipeline should be checked out.
* `credentialsId` - The Jenkins credentials containing user and password needed to access a private git repository. In case access to the repository containing the pipeline script is restricted the credentialsId of the credentials used for accessing the repository needs to be provided. The corresponding credentials needs to be configured in Jenkins accordingly.
* `path` - The path to the Jenkinsfile, inside the repository, to be loaded.
* `repoUrl` - The url to the git repository of the pipeline to be loaded.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `branch` |  |  |
| `credentialsId` |  |  |
| `path` |  |  |
| `repoUrl` |  |  |
| `script` |  |  |

## Side effects

none

## Exceptions

* `Exception`
  * If `repoUrl` is not provided.

## Example

```groovy
pipelineExecute repoUrl: "https://github.com/MyOrg/MyPipelineRepo.git", branch: 'feature1', path: 'path/to/Jenkinsfile', credentialsId: 'MY_REPO_CREDENTIALS'
```
