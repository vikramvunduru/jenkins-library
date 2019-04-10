# slackSendNotification

## Description

Sends notifications to the Slack channel about the build status.

Notification contains:

* Build status;
* Repo Owner;
* Repo Name;
* Branch Name;
* Jenkins Build Number;
* Jenkins Build URL.

## Prerequisites

* Installed and configured [Jenkins Slack plugin](https://github.com/jenkinsci/slack-plugin).

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `baseUrl` | no |  |  |
| `channel` | no |  |  |
| `color` | no | `${buildStatus == 'SUCCESS'?'#008000':'#E60000'}` | one of `good`, `warning`, `danger`, or any hex color code (eg. `#439FE0`) |
| `credentialsId` | no |  | Jenkins credentials id |
| `message` | no |  |  |
| `script` | yes |  |  |

* `baseUrl` - Allows overriding the Slack Plugin Integration Base Url specified in the global configuration.
* `channel` - Allows overriding of the default massaging channel from the plugin configuration.
* `color` - Defines the message color`color` defines the message color.
* `credentialsId` - The credentials id for the Slack token.
* `message` - Send a custom message into the Slack channel.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `baseUrl` |  | X |
| `channel` |  | X |
| `color` |  | X |
| `credentialsId` |  | X |
| `message` |  | X |
| `script` |  |  |

## Example

Usage of pipeline step:

```groovy
pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo "do something"
      }
    }
  }
  post {
    always {
      slackSendNotification script: this
    }
  }
}
```
