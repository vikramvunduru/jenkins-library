# transportRequestCreate

## Description

Creates

* a Transport Request for a Change Document on the Solution Manager (type `SOLMAN`) or
* a Transport Request inside an ABAP system (type`CTS`)

The id of the transport request is availabe via [commonPipelineEnvironment.getTransportRequestId()](commonPipelineEnvironment.md)

## Prerequisites

* **[Change Management Client 2.0.0 or compatible version](http://central.maven.org/maven2/com/sap/devops/cmclient/dist.cli/)** - available for download on Maven Central.
* Solution Manager version `ST720 SP08` or newer.

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `changeDocumentId` | yes |  |  |
| `changeManagement/changeDocumentLabel` | no | `ChangeDocument\s?:` |  |
| `changeManagement/clientOpts` | no |  |  |
| `changeManagement/credentialsId` | no | `CM` |  |
| `changeManagement/endpoint` | yes |  |  |
| `changeManagement/git/format` | no | `%b` |  |
| `changeManagement/git/from` | no | `origin/master` |  |
| `changeManagement/git/to` | no | `HEAD` |  |
| `changeManagement/rfc/developmentClient` | yes |  |  |
| `changeManagement/rfc/developmentInstance` | yes |  |  |
| `changeManagement/type` | no | `NONE` | `SOLMAN`, `CTS`, `RFC` |
| `description` | yes |  |  |
| `developmentSystemId` | yes |  |  |
| `script` | yes |  |  |
| `targetSystem` | yes |  |  |
| `transportType` | yes |  |  |
| `verbose` | no |  |  |

* `changeDocumentId` - The id of the change document to that the transport request is bound to. Typically this value is provided via commit message in the commit history. Only for `SOLMAN`.
* `changeManagement/changeDocumentLabel` - A pattern used for identifying lines holding the change document id.
* `changeManagement/clientOpts` - Additional options for cm command line client, e.g. JAVA_OPTS.
* `changeManagement/credentialsId` - The id of the credentials to connect to the Solution Manager. The credentials needs to be maintained on Jenkins.
* `changeManagement/endpoint` - The service endpoint, e.g. Solution Manager, ABAP System.
* `changeManagement/git/format` - Specifies what part of the commit is scanned. By default the body of the commit message is scanned.
* `changeManagement/git/from` - The starting point for retrieving the change document id.
* `changeManagement/git/to` - The end point for retrieving the change document id.
* `changeManagement/rfc/developmentClient` - AS ABAP client number. Only for `RFC`.
* `changeManagement/rfc/developmentInstance` - AS ABAP instance number. Only for `RFC`.
* `changeManagement/type` - Defines where the transport request is created, e.g. SAP Solution Manager, ABAP System.
* `description` - The description of the transport request. Only for `CTS`.
* `developmentSystemId` - The logical system id for which the transport request is created. The format is `<SID>~<TYPE>(/<CLIENT>)?`. For ABAP Systems the `developmentSystemId` looks like `DEV~ABAP/100`. For non-ABAP systems the `developmentSystemId` looks like e.g. `L21~EXT_SRV` or `J01~JAVA`. In case the system type is not known (in the examples provided here: `EXT_SRV` or `JAVA`) the information can be retrieved from the Solution Manager instance. Only for `SOLMAN`.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `targetSystem` - The system receiving the transport request. Only for `CTS`.
* `transportType` - Typically `W` (workbench) or `C` customizing. Only for `CTS`.
* `verbose` - Provides additional details. Only for `RFC`.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `changeDocumentId` |  |  |
| `changeManagement/changeDocumentLabel` |  | X |
| `changeManagement/clientOpts` |  | X |
| `changeManagement/credentialsId` |  | X |
| `changeManagement/endpoint` |  | X |
| `changeManagement/git/format` |  | X |
| `changeManagement/git/from` |  | X |
| `changeManagement/git/to` |  | X |
| `changeManagement/rfc/developmentClient` |  | X |
| `changeManagement/rfc/developmentInstance` |  | X |
| `changeManagement/type` |  | X |
| `description` |  | X |
| `developmentSystemId` |  | X |
| `script` |  |  |
| `targetSystem` |  | X |
| `transportType` |  | X |
| `verbose` |  | X |

The step is configured using a customer configuration file provided as
resource in an custom shared library.

```groovy
@Library('piper-lib-os@master') _

// the shared lib containing the additional configuration
// needs to be configured in Jenkins
@Library('foo@master') __

// inside the shared lib denoted by 'foo' the additional configuration file
// needs to be located under 'resources' ('resoures/myConfig.yml')
prepareDefaultValues script: this,
                             customDefaults: 'myConfig.yml'
```

Example content of `'resources/myConfig.yml'` in branch `'master'` of the repository denoted by
`'foo'`:

```yaml
general:
  changeManagement:
    changeDocumentLabel: 'ChangeDocument\s?:'
    cmClientOpts: '-Djavax.net.ssl.trustStore=<path to truststore>'
    credentialsId: 'CM'
    type: 'SOLMAN'
    endpoint: 'https://example.org/cm'
    git:
      from: 'HEAD~1'
      to: 'HEAD'
      format: '%b'
```

The properties configured in section `'general/changeManagement'` are shared between
all change managment related steps.

The properties can also be configured on a per-step basis:

```yaml
  [...]
  steps:
    transportRequestCreate:
      changeManagement:
        type: 'SOLMAN'
        endpoint: 'https://example.org/cm'
        [...]
```

The parameters can also be provided when the step is invoked. For examples see below.

## Return value

none

## Exceptions

* `AbortException`:
  * If the creation of the transport request fails.
* `IllegalStateException`:
  * If the change id is not provided.

## Example

```groovy
// SOLMAN
def transportRequestId = transportRequestCreate script:this,
                                                changeDocumentId: '001,'
                                                changeManagement: [
                                                  type: 'SOLMAN'
                                                  endpoint: 'https://example.org/cm'
                                                ]
// CTS
def transportRequestId = transportRequestCreate script:this,
                                                transportType: 'W',
                                                targetSystem: 'XYZ',
                                                description: 'the description',
                                                changeManagement: [
                                                  type: 'CTS'
                                                  endpoint: 'https://example.org/cm'
                                                ]
```
