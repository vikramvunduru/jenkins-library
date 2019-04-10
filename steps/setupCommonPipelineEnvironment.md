# setupCommonPipelineEnvironment

## Description

Initializes the [`commonPipelineEnvironment`](commonPipelineEnvironment.md), which is used throughout the complete pipeline.

!!! tip
   This step needs to run at the beginning of a pipeline right after the SCM checkout.
   Then subsequent pipeline steps consume the information from `commonPipelineEnvironment`; it does not need to be passed to pipeline steps explicitly.

## Prerequisites

* A **configuration file** with properties. The property values are used as default values in many pipeline steps.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `collectTelemetryData` | no | `true` |  |
| `configFile` | no |  |  |
| `script` | yes |  |  |

* `collectTelemetryData` - 
* `configFile` - Property file defining project specific settings.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `collectTelemetryData` | X |  |
| `configFile` |  |  |
| `script` |  |  |

## Side effects

none

## Exceptions

none

## Example

```groovy
setupCommonPipelineEnvironment script: this
```
