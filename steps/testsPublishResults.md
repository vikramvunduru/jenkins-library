# testsPublishResults

## Description

This step can publish test results from various sources.

## Prerequsites

* **test result files** - To use this step, there must be test result files available.
* installed plugins:
  * [junit](https://plugins.jenkins.io/junit)
  * [jacoco](https://plugins.jenkins.io/jacoco)
  * [cobertura](https://plugins.jenkins.io/cobertura)
  * [performance](https://plugins.jenkins.io/performance)

## Pipeline configuration

none

## Explanation of pipeline step

Usage of pipeline step:

```groovy
testsPublishResults(
  junit: [updateResults: true, archive: true],
  jacoco: [archive: true]
)
```

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `cobertura` | no | `[pattern:**/target/coverage/cobertura-coverage.xml, onlyStableBuilds:true, allowEmptyResults:true, archive:false, active:false]` | `true`, `false`, `Map` |
| `failOnError` | no |  | `true`, `false` |
| `jacoco` | no | `[pattern:**/target/*.exec, allowEmptyResults:true, archive:false, active:false]` | `true`, `false`, `Map` |
| `jmeter` | no | `[pattern:**/*.jtl, errorFailedThreshold:20, errorUnstableThreshold:10, errorUnstableResponseTimeThreshold:, relativeFailedThresholdPositive:0, relativeFailedThresholdNegative:0, relativeUnstableThresholdPositive:0, relativeUnstableThresholdNegative:0, modeOfThreshold:false, modeThroughput:false, nthBuildNumber:0, configType:PRT, failBuildIfNoResultFile:false, compareBuildPrevious:true, allowEmptyResults:true, archive:false, active:false]` | `true`, `false`, `Map` |
| `junit` | no | `[pattern:**/TEST-*.xml, updateResults:false, allowEmptyResults:true, archive:false, active:false]` | `true`, `false`, `Map` |
| `script` | yes |  |  |

* `cobertura` - Publishes code coverage with the [Cobertura plugin](https://plugins.jenkins.io/cobertura).
* `failOnError` - If it is set to `true` the step will fail the build if JUnit detected any failing tests.
* `jacoco` - Publishes code coverage with the [JaCoCo plugin](https://plugins.jenkins.io/jacoco).
* `jmeter` - Publishes performance test results with the [Performance plugin](https://plugins.jenkins.io/performance).
* `junit` - Publishes test results files in JUnit format with the [JUnit Plugin](https://plugins.jenkins.io/junit).
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

### junit

| parameter | mandatory | default | possible values |
| ----------|-----------|---------|-----------------|
| pattern | no | `'**/TEST-*.xml'` |  |
| archive | no | `false` | true, false |
| updateResults | no | `false` | true, false |
| allowEmptyResults | no | `true` | true, false |

### jacoco

| parameter | mandatory | default | possible values |
| ----------|-----------|---------|-----------------|
| pattern | no | `'**/target/*.exec'` |  |
| include | no | `''` | `'**/*.class'` |
| exclude | no | `''` | `'**/Test*'` |
| archive | no | `false` | true, false |
| allowEmptyResults | no | `true` | true, false |

### cobertura

| parameter | mandatory | default | possible values |
| ----------|-----------|---------|-----------------|
| pattern | no | `'**/target/coverage/cobertura-coverage.xml'` |  |
| archive | no | `false` | true, false |
| allowEmptyResults | no | `true` | true, false |
| onlyStableBuilds | no | `true` | true, false |

### jmeter

| parameter | mandatory | default | possible values |
| ----------|-----------|---------|-----------------|
| pattern | no | `'**/*.jtl'` |  |
| errorFailedThreshold | no | `20` |  |
| errorUnstableThreshold | no | `10` |  |
| errorUnstableResponseTimeThreshold | no | `` |  |
| relativeFailedThresholdPositive | no | `0` |  |
| relativeFailedThresholdNegative | no | `0` |  |
| relativeUnstableThresholdPositive | no | `0` |  |
| relativeUnstableThresholdNegative | no | `0` |  |
| modeOfThreshold | no | `false` | true, false |
| modeThroughput | no | `false` | true, false |
| nthBuildNumber | no | `0` |  |
| configType | no | `PRT` |  |
| failBuildIfNoResultFile | no | `false` | true, false |
| compareBuildPrevious | no | `true` | true, false |
| archive | no | `false` | true, false |
| allowEmptyResults | no | `true` | true, false |

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `cobertura` | X | X |
| `failOnError` |  | X |
| `jacoco` | X | X |
| `jmeter` | X | X |
| `junit` | X | X |
| `script` |  |  |

## Side effects

none

## Exceptions

none

## Example

```groovy
// publish test results with coverage
testsPublishResults(
  junit: [updateResults: true, archive: true],
  jacoco: [archive: true]
)
```

```groovy
// publish test results with coverage
testsPublishResults(
  junit: [pattern: '**/target/TEST*.xml', archive: true],
  cobertura: [pattern: '**/target/coverage/cobertura-coverage.xml']
)
```
