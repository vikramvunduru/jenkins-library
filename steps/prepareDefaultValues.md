# prepareDefaultValues

## Description

Loads the pipeline library default values from the file `resources/default_pipeline_environment.yml`.
Afterwards the values can be loaded by the method: `ConfigurationLoader.defaultStepConfiguration`

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `script` | yes |  |  |

* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `script` |  |  |

## Exceptions

None

## Example

```groovy
prepareDefaultValues()
```
