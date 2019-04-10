# mailSendNotification

## Description

Sends notifications to all potential culprits of a current or previous build failure and to fixed list of recipients.
It will attach the current build log to the email.

Notifications are sent in following cases:

* current build failed or is unstable
* current build is successful and previous build failed or was unstable

## Prerequsites

none

## Example

Usage of pipeline step:

```groovy
mailSendNotification script: this
```

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `buildResult` | no |  |  |
| `gitCommitId` | no |  |  |
| `gitSshKeyCredentialsId` | no |  | Jenkins credentials id |
| `gitUrl` | no |  |  |
| `notificationAttachment` | no | `true` | `true`, `false` |
| `notificationRecipients` | no |  |  |
| `notifyCulprits` | no | `true` | `true`, `false` |
| `numLogLinesInBody` | no | `100` |  |
| `projectName` | no |  |  |
| `script` | yes |  |  |
| `wrapInNode` | no |  | `true`, `false` |

* `buildResult` - Set the build result used to determine the mail template. default `currentBuild.result`
* `gitCommitId` - Only if `notifyCulprits` is set: Defines a dedicated git commitId for culprit retrieval. default `commonPipelineEnvironment.getGitCommitId()`
* `gitSshKeyCredentialsId` - Only if `notifyCulprits` is set: Credentials if the repository is protected.
* `gitUrl` - Only if `notifyCulprits` is set: Repository url used to retrieve culprit information. default `commonPipelineEnvironment.getGitSshUrl()`
* `notificationAttachment` - defines if the console log file should be attached to the notification mail.
* `notificationRecipients` - A space-separated list of recipients that always get the notification.
* `notifyCulprits` - Notify all committers since the last successful build.
* `numLogLinesInBody` - Number of log line which are included in the email body.
* `projectName` - The project name used in the email subject. default `currentBuild.fullProjectName`
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `wrapInNode` - Needs to be set to `true` if step is used outside of a node context, e.g. post actions in a declarative pipeline script. default `false`

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `buildResult` |  | X |
| `gitCommitId` |  | X |
| `gitSshKeyCredentialsId` | X | X |
| `gitUrl` |  | X |
| `notificationAttachment` |  | X |
| `notificationRecipients` |  | X |
| `notifyCulprits` |  | X |
| `numLogLinesInBody` |  | X |
| `projectName` |  | X |
| `script` |  |  |
| `wrapInNode` |  | X |

## Side effects

none

## Exceptions

none
