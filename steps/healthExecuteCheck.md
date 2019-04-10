# healthExecuteCheck

## Description

Calls the health endpoint url of the application.

The intention of the check is to verify that a suitable health endpoint is available. Such a health endpoint is required for operation purposes.

This check is used as a real-life test for your productive health endpoints.

!!! note "Check Depth"
    Typically, tools performing simple health checks are not too smart. Therefore it is important to choose an endpoint for checking wisely.

    This check therefore only checks if the application/service url returns `HTTP 200`.

    This is in line with health check capabilities of platforms which are used for example in load balancing scenarios. Here you can find an [example for Amazon AWS](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-healthchecks.html).

## Prerequisites

Endpoint for health check is configured.

!!! warning
    The health endpoint needs to be available without authentication!

!!! tip
    If using Spring Boot framework, ideally the provided `/health` endpoint is used and extended by development. Further information can be found in the [Spring Boot documenation for Endpoints](http://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html)

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `healthEndpoint` | no |  |  |
| `script` | yes |  |  |
| `testServerUrl` | yes |  |  |

* `healthEndpoint` - Optionally with `healthEndpoint` the health function is called if endpoint is not the standard url.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `testServerUrl` - Health check function is called providing full qualified `testServerUrl` to the health check.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `healthEndpoint` |  | X |
| `script` |  |  |
| `testServerUrl` |  | X |

## Example

Pipeline step:

```groovy
healthExecuteCheck testServerUrl: 'https://testserver.com'
```
