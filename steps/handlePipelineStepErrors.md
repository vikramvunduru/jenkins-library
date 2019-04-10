# handlePipelineStepErrors

## Description

Used by other steps to make error analysis easier. Lists parameters and other data available to the step in which the error occurs.

## Prerequisites

none

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `echoDetails` | no | `true` | `true`, `false` |
| `failOnError` | no | `true` | `true`, `false` |
| `libraryDocumentationUrl` | no | `https://sap.github.io/jenkins-library/` |  |
| `libraryRepositoryUrl` | no | `https://github.com/SAP/jenkins-library/` |  |
| `mandatorySteps` | no |  |  |
| `script` | yes |  |  |
| `stepName` | yes |  |  |
| `stepNameDoc` | no |  |  |
| `stepParameters` | yes |  |  |
| `stepTimeouts` | no |  |  |

* `echoDetails` - If it is set to true details will be output to the console. See example below.
* `failOnError` - Defines the behavior, in case an error occurs which is handled by this step. When set to `false` an error results in an "UNSTABLE" build result and the pipeline can continue.
* `libraryDocumentationUrl` - Defines the url of the library's documentation that will be used to generate the corresponding links to the step documentation.
* `libraryRepositoryUrl` - Defines the url of the library's repository that will be used to generate the corresponding links to the step implementation.
* `mandatorySteps` - Defines a list of mandatory steps (step names) which have to be successful (=stop the pipeline), even if `failOnError: false`
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `stepName` - Defines the name of the step for which the error handling is active. It will be shown in the console log.
* `stepNameDoc` - Defines the documented step, in case the documentation reference should point to a different step.
* `stepParameters` - Passes the parameters of the step which uses the error handling onto the error handling. The list of parameters is then shown in the console output.
* `stepTimeouts` - Defines a Map containing step name as key and timout in minutes in order to stop an execution after a certain timeout. This helps to make pipeline runs more resilient with respect to long running steps.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `echoDetails` |  |  |
| `failOnError` |  | X |
| `libraryDocumentationUrl` |  |  |
| `libraryRepositoryUrl` |  |  |
| `mandatorySteps` |  | X |
| `script` |  |  |
| `stepName` |  |  |
| `stepNameDoc` |  |  |
| `stepParameters` |  |  |
| `stepTimeouts` |  | X |

## Example

```groovy
handlePipelineStepErrors (stepName: 'executeHealthCheck', stepParameters: parameters) {
  def url = new Utils().getMandatoryParameter(parameters, 'url', null)
  def statusCode = curl(url)
  if (statusCode != '200')
    error "Health Check failed: ${statusCode}"
}
```

## Example console output

If `echoDetails` is set to true the following information will be output to the console:

1. Step beginning: `--- Begin library step: ${stepName}.groovy ---`
1. Step end: `--- End library step: ${stepName}.groovy ---`
1. Step errors:

```log
----------------------------------------------------------
--- An error occurred in the library step: ${stepName}
----------------------------------------------------------
The following parameters were available to the step:
***
${stepParameters}
***
The error was:
***
${err}
***
Further information:
* Documentation of step ${stepName}: .../${stepName}/
* Pipeline documentation: https://...
* GitHub repository for pipeline steps: https://...
----------------------------------------------------------
```
