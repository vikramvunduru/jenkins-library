# mtaBuild

## Description

Executes the SAP Multitarget Application Archive Builder to create an mtar archive of the application.

## Prerequisites

While using a custom docker file, ensure that the following tools are installed:

* **SAP MTA Archive Builder 1.0.6 or compatible version** - can be downloaded from [SAP Development Tools](https://tools.hana.ondemand.com/#cloud).
* **Java 8 or compatible version** - necessary to run the *MTA Archive Builder* itself and to build Java modules.
* **NodeJS installed** - the MTA Builder uses `npm` to download node module dependencies such as `grunt`.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `applicationName` | no |  |  |
| `buildTarget` | no | `NEO` | 'CF', 'NEO', 'XSA' |
| `dockerImage` | no | `ppiper/mta-archive-builder` |  |
| `dockerOptions` | no |  |  |
| `extension` | no |  |  |
| `mtaJarLocation` | no | `/opt/sap/mta/lib/mta.jar` |  |
| `script` | yes |  |  |

* `applicationName` - The name of the application which is being built. If the parameter has been provided and no `mta.yaml` exists, the `mta.yaml` will be automatically generated using this parameter and the information (`name` and `version`) from `package.json` before the actual build starts.
* `buildTarget` - The target platform to which the mtar can be deployed.
* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerOptions` - Docker options to be set when starting the container (List or String).
* `extension` - The path to the extension descriptor file.
* `mtaJarLocation` - The location of the SAP Multitarget Application Archive Builder jar file, including file name and extension. If it is not provided, the SAP Multitarget Application Archive Builder is expected on PATH.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `applicationName` |  | X |
| `buildTarget` |  | X |
| `dockerImage` |  | X |
| `dockerOptions` |  |  |
| `extension` |  | X |
| `mtaJarLocation` |  | X |
| `script` |  |  |

## Side effects

1. The file name of the resulting archive is written to the `commonPipelineEnvironment` with variable name `mtarFileName`.

## Exceptions

* `AbortException`:
  * If there is an invalid `buildTarget`.
  * If there is no key `ID` inside the `mta.yaml` file.

## Example

```groovy
def mtarFileName
dir('/path/to/FioriApp'){
  mtarFileName = mtaBuild script:this, buildTarget: 'NEO'
}
```
