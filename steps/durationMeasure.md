# durationMeasure

## Description

This step is used to measure the duration of a set of steps, e.g. a certain stage.
The duration is stored in a Map. The measurement data can then be written to an Influx database using step [influxWriteData](influxWriteData.md).

!!! tip
    Measuring for example the duration of pipeline stages helps to identify potential bottlenecks within the deployment pipeline.
    This then helps to counter identified issues with respective optimization measures, e.g parallelization of tests.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `measurementName` | no |  |  |
| `script` | yes |  |  |

* `measurementName` - Defines the name of the measurement which is written to the Influx database.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `measurementName` |  |  |
| `script` |  |  |

## Example

```groovy
durationMeasure (script: this, measurementName: 'build_duration') {
    //execute your build
}
```
